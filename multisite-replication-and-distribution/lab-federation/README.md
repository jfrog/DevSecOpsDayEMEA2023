### Prerequisites
A working training lab setup


For this lab you can choose between 2 options to test the setup of a federated repository : 
- One based on plain curl command (method 1)
- One using the JFrog CLI (method 2)

The choice really depend on your will to discover a setup method using plain linux command (no vendor lock-in), or if you want to discover the ease of use of the JFrog CLI.

# Method 1 : Use the JFrog CLI to generate an access token, and then call the API using cURL.

## Use CLI to generate an access_token
Create an access token for the user with the devsecopsday@jfrog.com username.
- Run
```
jf rt atc DevSecOpsDayLondon
```
Or 
- Run
```
jf rt atc devsecopsday@jfrog.com
```

## Use plain cURL 

 1. Replace {DevSecOpsDayLondon JPD host} with valid value
 2. Replace {DevSecOpsDayLondon JPD Edge host} with valid value
 3. Replace {DevSecOpsDayLondon Second JPD host} with valid value
 4. Replace <TOKEN> with the access-token generated from the previous step
 5. Copy and execute the below command in the terminal

```
curl --location --request PUT 'https://{DevSecOpsDayLondon JPD host}.jfrog.io/artifactory/api/repositories/jftd105lab3-maven-dev-local' \
-H "Content-Type: application/json" \
--header 'Authorization: Bearer <TOKEN>' \
--data '{
"key": "jftd105lab3-maven-dev-local",
"rclass": "federated",
"packageType": "maven",
"members": [
{
"url": "https://{DevSecOpsDayLondon JPD host}.jfrog.io/artifactory/jftd105lab3-maven-dev-local",
"enabled": true
},
{
"url": "https://{DevSecOpsDayLondon EDGE host}.jfrog.io/artifactory/jftd105lab3-maven-dev-local",
"enabled": true
},
{
"url": "https://{DevSecOpsDayLondon Second JPD host}.jfrog.io/artifactory/jftd105lab3-maven-dev-local",
"enabled": true
}
]
}'
```

# Method 2 : Use the Jfrog CLI's cURL wrapper
In this scenario, you won't have to generate the access token, as it will use your existing configuration.

## Make sure the JFrog CLI is configured to use your main JPD instance
- Run
```
jf c use swampup
```
## Then create the federated repositories using the `jf rt curl` command 

 1. Replace {DevSecOpsDayLondon JPD host} with valid value
 2. Replace {DevSecOpsDayLondon JPD Edge host} with valid value
 3. Replace {DevSecOpsDayLondon Second JPD host} with valid value

```
jf rt curl -XPUT /api/repositories/jftd105lab3-maven-dev-local -H "Content-Type: application/json" --data '{ "key": "jftd105lab3-maven-dev-local", "rclass": "federated", "packageType": "maven", "members": [ { "url": "https://{DevSecOpsDayLondon JPD host}.jfrog.io//artifactory/jftd105lab3-maven-dev-local", "enabled": true }, { "url": "https://{DevSecOpsDayLondon Edge host}/artifactory/jftd105lab3-maven-dev-local", "enabled": true }, { "url": "https://{DevSecOpsDayLondon Second JPD host}.jfrog.io/artifactory/jftd105lab3-maven-dev-local", "enabled": true } ] }'
```

## RUN SCRIPT[Optional]

If you have some difficulties to complete this lab, please edit `lab_3_federation_rescue.sh` and `template-federated-repo.json` to use your training environment domains names, and then run  
 
```
sh lab_3_federation_rescue.sh
```