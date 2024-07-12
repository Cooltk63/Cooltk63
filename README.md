package com.crs.cs.JwtAuth;

import com.crs.cs.Model.UserMaster;
import com.crs.cs.Service.UserMasterServiceImpl;
import io.jsonwebtoken.Claims;
import io.jsonwebtoken.ExpiredJwtException;
import io.jsonwebtoken.Jwts;
import io.jsonwebtoken.SignatureAlgorithm;
import io.jsonwebtoken.io.Decoders;
import io.jsonwebtoken.security.Keys;
import jakarta.servlet.http.HttpServletRequest;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.security.authentication.UsernamePasswordAuthenticationToken;
import org.springframework.security.core.context.SecurityContextHolder;
import org.springframework.security.web.authentication.WebAuthenticationDetailsSource;
import org.springframework.stereotype.Component;

import java.security.Key;
import java.util.Date;
import java.util.HashMap;
import java.util.Map;
import java.util.function.Function;
import java.util.logging.Logger;

@Component
public class JwtUtil {

    static Logger log = Logger.getLogger(JwtUtil.class.getName());
    private final String SECRET_KEY = "thisIsTheMostSecretKeytcs123springframeworkspringframeworkspringframework";
    @Autowired
    private UserMasterServiceImpl userMasterServiceImpl;

//    public String extractUsername(String token) {
//        return extractClaim(token, Claims::getSubject);
//    }

    private Boolean isTokenExpired(String token) {
        log.info(" <==========isTokenExpired Function Called ==========>:: ");
        return extractExpiration(token).before(new Date());
    }

    public Date extractExpiration(String token) {

        log.info(" <==========extractExpiration Function Called ==========>:: ");
        return extractClaim(token, Claims::getExpiration);
    }

    public <T> T extractClaim(String token, Function<Claims, T> claimsResolver) {
        final Claims claims = extractAllClaims(token);
        log.info(" <==========extractClaim Function Called ==========>:: ");
        return claimsResolver.apply(claims);
    }

    private Claims extractAllClaims(String token) {
        log.info(" <==========extractAllClaims Function Called ==========>:: ");
        System.out.println("Calling the extractAllClaims Method @@@@@@");
//        String GetUserName=CalimuserName(token);
//        System.out.println("Claim  UserName:"+GetUserName);
        return Jwts.parser().setSigningKey(SECRET_KEY).parseClaimsJws(token).getBody();
    }



    public String generateToken(UserMaster userMaster) {
        log.info("<==========GenerateToken Function Called ==========>::  ");

        // Added the login Users Details inside JWT Token
        Map<String, Object> claims = new HashMap<>();
        claims.put("pf_number", userMaster.getPf());
        claims.put("first_name", userMaster.getFirst_name());
        claims.put("middle_name", userMaster.getMiddle_name());
        claims.put("last_name", userMaster.getLast_name());
        claims.put("branch_code", userMaster.getBranch_code());
        claims.put("mobile_number", userMaster.getMobile_number());
        claims.put("email_id", userMaster.getEmail_id());
        claims.put("user_role", userMaster.getUserRole());
        claims.put("status", userMaster.getStatus());
        claims.put("created_dt", userMaster.getCreated_dt());
        claims.put("created_by_fk", userMaster.getCreated_by_fk());

        // Token Expires After 1 Hrs
        return Jwts.builder().setClaims(claims).setSubject("TOKEN").setIssuedAt(new Date(System.currentTimeMillis())).setExpiration(new Date(System.currentTimeMillis() + 1000 * 60 * 60 * 1)).signWith(SignatureAlgorithm.HS256, SECRET_KEY).compact();
    }

    public Boolean validateToken(String token, int PFID) {
        log.info(" <==========validateToken Function Called ==========>:: ");
        final int usernameFromToken = extractPfNumber(token);
        return (usernameFromToken == PFID && !isTokenExpired(token));
    }

    // Extract the Users PfNumber from JWt Token
    public int extractPfNumber(String token) {
        return extractClaim(token, claims -> claims.get("pf_number", Integer.class));
    }

    // Extract the Users FirstName from JWt Token
    public String extractFirstName(String token) {
        return extractClaim(token, claims -> claims.get("first_name", String.class));
    }

    // Extract the Users LastName from JWt Token
    public String extractLastName(String token) {
        return extractClaim(token, claims -> claims.get("last_name", String.class));
    }

    // Extract the Users MiddleName from JWt Token
    public String extractMiddleName(String token) {
        return extractClaim(token, claims -> claims.get("middle_name", String.class));
    }

    // Extract the Users BranchCode from JWt Token
    public String extractBranchCode(String token) {
        return extractClaim(token, claims -> claims.get("branch_code", String.class));
    }
    // Extract the Users extractEmailId from JWt Token
    public String extractEmailId(String token) {
        return extractClaim(token, claims -> claims.get("email_id", String.class));
    }
    // Extract the Users extractUserRole from JWt Token
    public String extractUserRole(String token) {
        return extractClaim(token, claims -> claims.get("user_role", String.class));
    }

    // Extract the Users extractStatus from JWt Token
    public String extractStatus(String token) {
        return extractClaim(token, claims -> claims.get("status", String.class));
    }

    // Extract the Users extractCreatedByFk from JWt Token
    public String extractCreatedByFk(String token) {
        return extractClaim(token, claims -> claims.get("created_by_fk", String.class));
    }
    // Extract the Users extractCreatedDt from JWt Token
    public String extractCreatedDt(String token) {
        return extractClaim(token, claims -> claims.get("created_dt", String.class));
    }


    public Map<String, Object> getTokenData(String token) {
        Map<String, Object> tokenData = new HashMap<>() ;
        tokenData.put("pf_number", extractPfNumber(token));
        tokenData.put("first_name", extractFirstName(token));
        tokenData.put("middle_name", extractMiddleName(token));
        tokenData.put("last_name", extractLastName(token));
        tokenData.put("branch_code", extractBranchCode(token));
        tokenData.put("email_id", extractEmailId(token));
        tokenData.put("user_role", extractUserRole(token));
        tokenData.put("status", extractStatus(token));
        tokenData.put("created_by_fk", extractCreatedByFk(token));
        tokenData.put("created_dt", extractCreatedDt(token));
        tokenData.put("created_by_fk", extractCreatedByFk(token));
        return tokenData;
    }



    public String IsTokenValidate(HttpServletRequest request, String token) {

        int extractPfNumber = extractPfNumber(token);
        String extractFirstName = extractFirstName(token);
        String extractLastName = extractLastName(token);

        System.out.println("@@@ extractPfNumber values ::: " + extractPfNumber);
        System.out.println("@@@ extractFirstName values ::: " + extractFirstName);
        System.out.println("@@@ extractLastName values ::: " + extractLastName);

        final String authorizationHeader = request.getHeader("Authorization");

        int PF_Number = 0;
        String jwt = null;
        String requestResult = "Token is invalid";

        if (authorizationHeader != null && authorizationHeader.startsWith("Bearer")) {

            System.out.println("inside filter IF ");
            jwt = authorizationHeader.substring(7);
            try {
                PF_Number = extractPfNumber(jwt);
                System.out.println("PFID WE GET:"+PF_Number);
            } catch (ExpiredJwtException e) {
                System.out.println("JWT Token has expired");
            } catch (Exception e) {
                System.out.println("Error while extracting username from JWT");
            }
        }

//        request.setAttribute("token", "Invalid");
        if (PF_Number != 0 /*&& SecurityContextHolder.getContext().getAuthentication() == null*/) {

            System.out.println("Inside username != null ");

//            UserDetails userDetails = this.userDetailsService.loadUserByUsername(username);

            UserMaster userMaster = this.userMasterServiceImpl.getCustomUserData(PF_Number);

            System.out.println("Loading loadUserByUsername :" + PF_Number);

            if (validateToken(jwt, userMaster.getPf())) {

                System.out.println("Inside ValidateToken ::::" + userMaster.getPf());

                UsernamePasswordAuthenticationToken usernamePasswordAuthenticationToken = new UsernamePasswordAuthenticationToken(userMaster, null);
                usernamePasswordAuthenticationToken.setDetails(new WebAuthenticationDetailsSource().buildDetails(request));
                SecurityContextHolder.getContext().setAuthentication(usernamePasswordAuthenticationToken);

//                request.setAttribute("token", "valid");
                requestResult = "Token is valid";
            }
        }
        return requestResult;
    }
}
