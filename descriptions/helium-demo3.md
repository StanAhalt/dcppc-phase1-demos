# Helium Demo 3: Log-in newly registered users from another stack

The steps are similar as
[helium demo 2: Register users via a Web interface](helium-demo2.html) above
recognizing that the authentication of stacks is based on OAuth with
credentials common to these stacks.
* A Client_ID key, Client_Secret
key, and Redirect_URL are provided on the RemoteStack.
* A “Login via
CommonsShare” button is presented on the RemoteStack. 
* Selecting the
button takes the user to the following URL for CommonsShare login:
https://www.commonsshare.org/o/authorize/?redirect_uri=${ENCODED_REDIRECT_URL}&client_id=${CLIENT_ID}&response_type=code.
* Once user logs in CommonsShare, CommonsShare takes user back to
REDIRECT_URL and append a parameter code=${CODE} to the end
REDIRECT_URL/?code=${CODE}
https://remotestack.org/hs-oauth/?code=${CODE}.  RemoteStack backend
should extract this CODE and make a POST request to
https://www.commonsshare.org/o/token/. 
* Note: this CODE has a very
short life time. So the POST request should be made ASAP.  The
response of this POST request is a json string contains ACCESS_TOKEN,
and RemoteStack should save it on backend {"access_token":
${ACCESS_TOKEN}, "token_type": "Bearer", "expires_in": 2592000,
"refresh_token": ${REFRESH_CODE}, "scope": "read write"}.  RemoteStack
uses this ACCESS_TOKEN to access CommonsShare REST API (or
cs_restclient).
