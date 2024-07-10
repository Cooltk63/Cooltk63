package com.crs.cs.JwtAuth;

import io.jsonwebtoken.Claims;
import io.jsonwebtoken.Jwts;
import io.jsonwebtoken.SignatureAlgorithm;
import org.springframework.stereotype.Component;

import java.util.Date;
import java.util.HashMap;
import java.util.Map;
import java.util.function.Function;
import java.util.logging.Logger;

@Component
public class JwtUtil {

    static Logger log = Logger.getLogger(JwtUtil.class.getName());

    private final String SECRET_KEY = "thisIsTheMostSecretKeytcs123springframeworkspringframeworkspringframework";

    public String extractUsername(String token) {
        return extractClaim(token, Claims::getSubject);
    }

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


    public String generateToken(String username) {
        log.info("<==========GenerateToken Function Called ==========>::  ");


//        Here We Have to Put User Details Inside the Token
        Map<String, Object> claims = new HashMap<>();
        String Address = "SBI GITC";
        String branch = "SBI GITC";
        claims.put("userName", username);
        claims.put("address", Address);
        claims.put("branch", branch);


        return Jwts.builder().setClaims(claims).setSubject(username).setIssuedAt(new Date(System.currentTimeMillis()))
                .setExpiration(new Date(System.currentTimeMillis() + 1000 * 60 * 60 * 10))
                .signWith(SignatureAlgorithm.HS256, SECRET_KEY).compact();
    }

    public Boolean validateToken(String token, String username) {
        log.info(" <==========validateToken Function Called ==========>:: ");
        final String usernameFromToken = extractUsername(token);
        return (usernameFromToken.equals(username) && !isTokenExpired(token));
    }
}
