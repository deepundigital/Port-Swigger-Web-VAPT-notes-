# LAB NAME :
    JWT authentication bypass via algorithm confusion

# Description : 
     This lab uses a JWT-based mechanism for handling sessions. It uses a robust RSA key pair to sign and verify tokens. However, due to implementation flaws, this mechanism is vulnerable to algorithm confusion attacks.

    To solve the lab, first obtain the server's public key. This is exposed via a standard endpoint. Use this key to sign a modified session token that gives you access to the admin panel at /admin, then delete the user carlos.

    You can log in to your own account using the following credentials: wiener:peter
# What is Algorithm Confusion ?
    Its occur when the server support  both the algorithm RS256 and HS256 and application server trust the given alg value . An attacker use this trust use the RSA public key as HMAC secret to generate a valid signature.
#  Objective : 
    Delete carlos user using gain the access of admin by exploiting this vulnerablity.

# Methodology :
     
    # Thinking : This vulnerablity genrally exist because in the implementation of JWT Authentication when application trust alg value in the JWT header.If attacker change the algo value RS256 to HS256 , then the verification library treats the RSA public key as HMAC secret that why attacker able to forge the jwt token.
    publicKey = <public-key-of-server>;
    token = request.getCookie("session");
    verify(token, publicKey);

    When the user use symmetric algorithm like HS256 there  verify() library treat public key as HMAC secret . Due to this flawed assumption attacker can 
    can sign using public key.
    
    Note : When we want try to access the admin panel using the /admin endpoint there we seen as message admin panel is access only by administrator that's why we went to access the admin account for accessing the admin panel.

    1. Obtain the server's public key :
        Sometime developer can expose the endpoint like jwks.json or /.well-known/jwks.json this may contain the jwk called as keys.
        keys":[{"kty":"RSA","e":"AQAB","use":"sig","kid":"7b633d29-8d06-408d-bc1e-4dec2af02390","alg":"RS256","n":"xxaA3f-VUaYOQXAzGw8OV7q1ENB6zo5-CVuTjF7fzRZCDzkTHoiEjsd9copJ-Qrxb5Ik5hQhuSQv0E6qmERqdg-HB6ZJ3zQ9AXSqjoIir2R-WvHko56fiFHFNRcmtTHbjnRsk6Apvq_vG0Z9OWTihsbb_KCXiGUASEGvreSIvB3uVsf78BC4Tb96T01sMRQj229B8HF0MMhhu4bj9zZrIfBoc_q7a0_E98kk84rT1Gie1P-hr1Q4iUz4obYdIHNWf_Ct9jh5PWdE2kyRW1sLNylBSp1HoO3bd6sK_c0yd7toCEV400xISyPKEphNJ33sxONKWnGqarkScSGONltlsQ"}]
    
    2. Convert this public key into HMAC secret :
        (i). We use jwt editor extension of Burpsuite and go to the jwt editor.
        (ii). In there we open the (new RSA key) and paste the key on that and copy the genrated key as PEM.

                    -----BEGIN PUBLIC KEY-----
        MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAxxaA3f+VUaYOQXAzGw8O
        V7q1ENB6zo5+CVuTjF7fzRZCDzkTHoiEjsd9copJ+Qrxb5Ik5hQhuSQv0E6qmERq
        dg+HB6ZJ3zQ9AXSqjoIir2R+WvHko56fiFHFNRcmtTHbjnRsk6Apvq/vG0Z9OWTi
        hsbb/KCXiGUASEGvreSIvB3uVsf78BC4Tb96T01sMRQj229B8HF0MMhhu4bj9zZr
        IfBoc/q7a0/E98kk84rT1Gie1P+hr1Q4iUz4obYdIHNWf/Ct9jh5PWdE2kyRW1sL
        NylBSp1HoO3bd6sK/c0yd7toCEV400xISyPKEphNJ33sxONKWnGqarkScSGONltl
        sQIDAQAB
                    -----END PUBLIC KEY-----
        (iii). Go to the decoder where we encode the PEM key into base64 encoding.
        LS0tLS1CRUdJTiBQVUJMSUMgS0VZLS0tLS0KTUlJQklqQU5CZ2txaGtpRzl3MEJBUUVGQUFPQ0FROEFNSUlCQ2dLQ0FRRUF4eGFBM2YrVlVhWU9RWEF6R3c4TwpWN3ExRU5CNnpvNStDVnVUakY3ZnpSWkNEemtUSG9pRWpzZDljb3BKK1FyeGI1SWs1aFFodVNRdjBFNnFtRVJxCmRnK0hCNlpKM3pROUFYU3Fqb0lpcjJSK1d2SGtvNTZmaUZIRk5SY210VEhiam5Sc2s2QXB2cS92RzBaOU9XVGkKaHNiYi9LQ1hpR1VBU0VHdnJlU0l2QjN1VnNmNzhCQzRUYjk2VDAxc01SUWoyMjlCOEhGME1NaGh1NGJqOXpacgpJZkJvYy9xN2EwL0U5OGtrODRyVDFHaWUxUCtocjFRNGlVejRvYllkSUhOV2YvQ3Q5amg1UFdkRTJreVJXMXNMCk55bEJTcDFIb08zYmQ2c0svYzB5ZDd0b0NFVjQwMHhJU3lQS0VwaE5KMzNzeE9OS1duR3FhcmtTY1NHT05sdGwKc1FJREFRQUIKLS0tLS1FTkQgUFVCTElDIEtFWS0tLS0tCg==

        (iv). Go back to the jwt editor and click on the new symmteric key and genrate new symmeteric key and change the k parameter value with this encoded key.
    
    3. Modifying the Jwt token 
        Here we change algo RS256 to HS256 and change username to administrator and sign using HMAC key which we genrate in the jwt editor.
        
        Thinking : Why its work ? 
                    Because HS256 uses the same secret for signing and verification and its application incorrectly treats the RSA public key as HMAC secret.    
    4. Delete the carlos user 
        After modifying the jwt token we send a request from repater on the admin panel there we access then admin panel.
        After that we seen where we find the carlos user endpoint on the html source code /admin/delete?username=carlos and send this request.
        Congratulations : We successfully delete the carlos user and solved the lab.

# Impact :
    Any unauthorised user can perform administrative actions and lead to account takeover and full application compromise.        