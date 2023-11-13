# REPLICATION LAB  - Build, Properties and Replication

## Prerequisites
A working training lab setup


# Configure a Push Replication

In this first part of the lab, we will guide you to set up a push replication between your main and secondary JPDs.
The main will be the source, and the secondary the destination.
You will use the [JFrog CLI](https://jfrog.com/getcli/) to perform most of the setup.

## Create the replication template

First we will create a replication template, that contains the specification of the replication that we want to set up.

- Run

  ```
  jf rt replication-template template-push.json
  ```

  or

  ```
   jf rt rplt template-push.json
  ```

  - Select replication job type (press Tab for options): `push`
  - Enter source repo key > `jftd105lab1-maven-dev-local`         
  - Enter target repo key > `jftd105lab1-maven-dev-local`   
  - Enter target Server server id > swampupsecond
  - Enter cron expression for frequency (for example, 0 0 12 * * ? will replicate daily) > `*/10 * * * * ?`
  - You can type ":x" at any time to save and exit.
  - Select the next property > `enabled`
  - Insert the value for enabled (press Tab for options): > `true`
  - Select the next property > `enableEventReplication`
  - Insert the value for enableEventReplication (press Tab for options): > `true`
  - Select the next property > `syncDeletes`
  - Insert the value for syncDeletes (press Tab for options): > `true`
  - Select the next property > `syncProperties`
  - Insert the value for syncProperties (press Tab for options): > `true`
  - Select the next property > `syncStatistics`
  - Insert the value for syncStatistics (press Tab for options): > `true`
  - Select the next property > `:x`

- Validate template template-push.json is created successfully. `ls -la`
- Review the generated template

      ```json
      {
        "cronExp": "*/10 * * * * ?",
        "enableEventReplication": "true",
        "enabled": "true",
        "repoKey": "jftd105lab1-maven-dev-local",
-       "serverId":"swampupedge",
-       "targetRepoKey":"jftd105lab1-maven-dev-local"
        "syncDeletes": "true",
        "syncProperties": "true",
        "syncStatistics": "true"
      }
      ```
Now, we will use this replication definition template, to effectively create a replication.
- Run
  ```
   jf rt replication-create template-push.json
  ```

<br />

Check using the ui, the results of this command.

# Configure a Pull Replication

We will follow basically the same steps as above, but this time to set up a pull replication.

## Create the replication template

- Run

  ```
   jf rt replication-template template-pull.json
  ```

  or

  ```
  jf rt rplt template-pull.json
  ```

  - Select replication job type (press Tab for options): `pull`
  - Enter source repo key > `jftd105lab1-maven-remote`
  - Enter cron expression for frequency (for example, 0 0 12 * * ? will replicate daily) > `*/10 * * * * ?`
  - You can type ":x" at any time to save and exit.
  - Select the next property > `enabled`
  - Insert the value for enabled (press Tab for options): > `true`
  - Select the next property > `enableEventReplication`
  - Insert the value for enableEventReplication (press Tab for options): > `true`
  - Select the next property > `syncDeletes`
  - Insert the value for syncDeletes (press Tab for options): > `true`
  - Select the next property > `syncProperties`
  - Insert the value for syncProperties (press Tab for options): > `true`
  - Select the next property > `syncStatistics`
  - Insert the value for syncStatistics (press Tab for options): > `true`
  - Select the next property > `:x`

- Validate template template-pull.json is created successfully. `ls -la`
- View template

```json
      {
        "cronExp": "*/10 * * * * ?",
        "enableEventReplication": "true",
        "enabled": "true",
        "repoKey": "jftd105lab1-maven-remote",
        "syncDeletes": "true",
        "syncProperties": "true",
        "syncStatistics": "true"
      }
```

Now you must edit the template to specify the orgini of your remote repository.
We will choose as origin repository `jftd105lab1-maven-dev-local-for-pull`.

It should be similar to (edit of the main JPD domain name) :
```json
      {
        "cronExp": "*/10 * * * * ?",
        "enableEventReplication": "true",
        "enabled": "true",
        "repoKey": "jftd105lab1-maven-remote",
        "targetRepoKey":"https://dsod23lom1YY.jfrog.io/jftd105lab1-maven-dev-local-for-pull",
        "syncDeletes": "true",
        "syncProperties": "true",
        "syncStatistics": "true"
      }
```

Now we will create the pull replication, using this template.

- Set the swampupsecond instance as active
```
jf c use swampupsecond
```
- Apply the pull replication details
  ```
  jf rt replication-create template-pull.json
  ```
- Verify by logging to swampupsecond instance
- Go to Administration Tab-->Repositories-->Repositories-->Remote 
- Select the repository 
- Verify URL pointing to Swampup main JPD and verify replication details in "Replication" tab
<br />


## RUN SCRIPT[Optional]
If things goes bad, or if you are out of time for this lab, run this helper script

- Run 
```
sh lab-1-replication-rescue.sh
```


