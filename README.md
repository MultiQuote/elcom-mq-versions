# elcom-mq-versions
Encapsulates the full version information for MultiQuote front and backends across all projects for a particular full environment build.

Process
To deploy a new version of Product UI/API on QA/Prod it is a two step process:

1) Create a new overall build file i.e builds/54.json. This is done via the Jenkins job cme-create-version

2) Deploy via the appropriate Jenkins job the wars/zips for QA or Prod. This takes in the id of the build above. It reads the json file to find what versions were in that build and then deploys those.

## Creating a New Build
The Jenkins job cme-create-version is set up so when you click build it:

1. Retrieves the latest version number of all the projects for Product UI and API

2. Constructs a json file with this info, along with the date and the build id of the job as the overall version number

3. Saves this json file to a file named builds/latest.json, overwriting its content

4. Also saves the same file to builds/54.json where 54 is the build id of the job

5. Commits this back to git and pushes with a tag of the overall version number e.g. 54


## Deploying Build
Note: this is only of use for QA/Prod as Dev is continually deploying each of the APIs/UI whenever there is a change.

Each environment will have its own Jenkins job to deploy all APIs/UI relating to the above overall version number which acts as follows:

You enter the version number or latest when running the job
The job will read the appropriate json file e.g. for 54: builds/54.json or for latest builds/latest.json
The version numbers of each individual project will be read and the war/zip file downloaded from AWS
These will then be deployed to the correct environment
Finally the json file itself will be uploaded to the server to allow the UI to read the deployed version number
