 public String IsTokenValidate(HttpServletRequest request,String token)
    {

        String extractPfNumber= extractPfNumber(token);
        String extractFirstName= extractFirstName(token);
        String extractLastName= extractLastName(token);

        System.out.println("@@@ extractPfNumber values ::: "+extractPfNumber);
        System.out.println("@@@ extractFirstName values ::: "+extractFirstName);
        System.out.println("@@@ extractLastName values ::: "+extractLastName);

        final String authorizationHeader = request.getHeader("Authorization");

        String username = null;
        String jwt = null;
        String requestResult="Token is invalid";

        if (authorizationHeader != null && authorizationHeader.startsWith("Bearer")) {

            System.out.println("inside filter IF ");
            jwt = authorizationHeader.substring(7);
            try {
                username = extractUsername(jwt);
            } catch (ExpiredJwtException e) {
                System.out.println("JWT Token has expired");
            } catch (Exception e) {
                System.out.println("Error while extracting username from JWT");
            }
        }

        request.setAttribute("token", "Invalid");
        if (username != null && SecurityContextHolder.getContext().getAuthentication() == null) {

            System.out.println("Inside username != null ");

            UserDetails userDetails = this.userDetailsService.loadUserByUsername(username);

            System.out.println("Loading loadUserByUsername :"+username);

            if (validateToken(jwt, userDetails.getUsername())) {

                System.out.println("Inside ValidateToken ::::"+userDetails.getUsername());

                UsernamePasswordAuthenticationToken usernamePasswordAuthenticationToken = new UsernamePasswordAuthenticationToken(
                        userDetails, null, userDetails.getAuthorities());
                usernamePasswordAuthenticationToken
                        .setDetails(new WebAuthenticationDetailsSource().buildDetails(request));
                SecurityContextHolder.getContext().setAuthentication(usernamePasswordAuthenticationToken);

//                request.setAttribute("token", "valid");
                requestResult="Token is valid";
            }
        }
        return requestResult;
    }
