package com.crs.cs.Service;


import com.crs.cs.Model.User;
import com.crs.cs.Model.UserMaster;
import com.crs.cs.Repository.UserRepository;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.security.core.userdetails.UserDetails;
import org.springframework.security.core.userdetails.UserDetailsService;
import org.springframework.security.core.userdetails.UsernameNotFoundException;
import org.springframework.stereotype.Service;

import java.util.ArrayList;
import java.util.logging.Logger;

@Service
public class CustomUserDetailsService implements UserDetailsService {

    @Autowired
     UserRepository userRepository;

    static Logger log = Logger.getLogger(CustomUserDetailsService.class.getName());

    @Override
    public UserDetails loadUserByUsername(String username) throws UsernameNotFoundException {
        log.info("Inside Load dby User ");

	// Here we write our service method to get the user from Backend
//        User user = userRepository.findByUsername(username);

       


        User user=new User();
        user.setUsername("Tushar");
        user.setPassword("Tk123");
//        user.setEnabled(true);

        log.info("User :"+user);
//        log.info("User :"+ user.getUsername().equalsIgnoreCase(username));
        if (user == null || !user.getUsername().equalsIgnoreCase(username)) {
            return null;
//
        }
        /*return new org.springframework.security.core.userdetails.User(user.getUsername(), user.getPassword(),
                user.isEnabled(), true, true, true, new ArrayList<>()); // Adjust the authorities as needed*/

        return new org.springframework.security.core.userdetails.User(user.getUsername(), user.getPassword(),
                true, true, true, true, new ArrayList<>()); // Adjust the authorities as needed
    }
    
    
    public UserMaster loadUserById(Long id) {
        UserMaster userMaster=userRepository.findAllById(id);
        
        if(userMaster== null || userMaster.getPf_number() !=id)
        {
            System.out.println("No Data Received in UserMaster");
            return  null;
        }
        return new org.springframework.security.core.userdetails.User(userMaster.getFirst_name(), userMaster.getMiddle_name(), userMaster.getLast_name(),userMaster.getUser_role(),
                , true, true, true, new ArrayList<>()); // Adjust the authorities as needed
        
    }
}

