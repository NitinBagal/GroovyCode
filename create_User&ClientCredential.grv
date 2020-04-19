import groovy.json.JsonOutput
import javax.xml.bind.DatatypeConverter;
import io.jsonwebtoken.Jwts;
import io.jsonwebtoken.Jwt;
import io.jsonwebtoken.Claims;
import io.jsonwebtoken.Header;
import io.jsonwebtoken.ExpiredJwtException;
import java.security.PrivateKey;
import java.security.spec.PKCS8EncodedKeySpec;
import java.security.interfaces.RSAPublicKey;
import java.security.KeyFactory;
import io.jsonwebtoken.SignatureAlgorithm;

//import groovy.json.JsonSlurper;
//import javax.crypto.Cipher;
//import groovy.json.*
//import java.security.KeyPair;
//import java.security.KeyPairGenerator;
//import java.security.NoSuchAlgorithmException;
//import java.security.PublicKey;

//Created By Nitin Bagal on 17-Apr-2020

//Declaring variables
tokenUrl = 'https://<RSSO Server with port>/rsso/oauth2/token';
def clientId = '<CLIENT_ID>';
def clientSecret = 'CLIENT_SECRETE';
def PRIVATE_KEY = '<CLIENT_SECRETE>';
issuer = '<RSSO Server with port>/rsso'
//log.info (issuer)
def issuerName = '<Anything>'

//concatenate clientId and clientSecret, then encode it.
def con = clientId + ":" + clientSecret
//log.info ("concatenation of clientId and Secret: " +con)
String token = con.bytes.encodeBase64 ().toString()
//log.info ("token: " +token)

testRunner.testCase.setPropertyValue("oauthToken",token)

username = "myssouser";
//def jwtExpirySeconds = '60';

//Decode Private Key 
 KeyFactory kf = KeyFactory.getInstance("RSA");

        PKCS8EncodedKeySpec keySpecPKCS8 = new PKCS8EncodedKeySpec(Base64.getDecoder().decode(PRIVATE_KEY));
        PrivateKey privKey = kf.generatePrivate(keySpecPKCS8);

  //      log.info ("private Key: " +privKey)

//Get the current time and set expiry time

 long nowMs = System.currentTimeMillis();
 //log.info ("curretTime: " +nowMs)
            long expMs = nowMs + 9000000;
            //long newNow = nowMs - 90000000;
            Date now = new Date(nowMs);
            Date exp = new Date(expMs); 
		//log.info ("Minus time" +newNow)
        	//log.info ("Issue Date: " +now) 
        	//log.info ("Expiry Date: " +exp) 

//Creating user assertion
String userJWT = Jwts.builder()
		.setSubject(username)
		.setAudience(issuer)
		.setIssuer(issuerName)
		.setExpiration(exp)
		.setIssuedAt(now)
		.signWith(SignatureAlgorithm.RS256, privKey)
		.compact();

//log.info ("UserJWT: " +userJWT)
testRunner.testCase.setPropertyValue("user_assertion",userJWT)


//Create client assertion
String clientJWT = Jwts.builder()
		.setSubject(clientId)
		.setAudience(issuer)
		.setIssuer(clientId)
		.setExpiration(exp)
		.setIssuedAt(now)
		.signWith(SignatureAlgorithm.RS256, privKey)
		.compact();

//log.info ("ClientJWT: " +clientJWT)
testRunner.testCase.setPropertyValue("client_assertion", clientJWT)
