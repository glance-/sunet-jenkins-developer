### Setup
Clone this repo.
Create a symlink to or clone the repo [bootstrap-docker-builds](https://github.com/SUNET/bootstrap-docker-builds) in /path/to/sunet-jenkins-developer/bootstrap-docker-builds.
```bash
git clone git@github.com:SUNET/bootstrap-docker-builds.git /path/to/sunet-jenkins-developer/
```
Clone the docker-jenkins-job repo and build its containers with the docker-wrapper
to turn docker push into a nop, so you won't fail those jobs or affect the real registry:
```bash
git clone git@github.com:SUNET/docker-jenkins-job.git /path/to/docker-jenkins-job
make -C /path/to/docker-jenkins-job -j all_extra_job_docker_wrapper
```
Run start.sh.
```bash
./start.sh
```

### How to run (as) in prod?
* Clone this repo
* Create a github Oauth App at https://github.com/settings/developers and put GITHUB_CLIENT_ID GITHUB_CLIENT_SECRET in .env
* Create and put a SLACK_TOKEN in .env
* Create and put a GITHUB_TOKEN in .env
* Write a list of admins github usernames as GITHUB_ADMIN_USERNAMES in .env
* Get the certificates for the docker host to run agents on and point out those files with DOCKER_SERVER_CA_CERTIFICATE DOCKER_CLIENT_CERTIFICATE DOCKER_CLIENT_KEY in .env
* Configure the DOCKER_URI pointing to the right host in .env
* Get the certificates for the service and point out that bundle with DEHYDRATED_FULLCHAIN and DEHYDRATED_KEY in .env
* And run it as:
```bash
./bin/docker-compose -f jenkins_compose/compose.yml -f jenkins_compose/prod.yml
```

#### Test build your own repos
Create a folder, and in that folder create a copy of the bootstrap-docker-builds
job, and edit its enviorment.
(New item, Copy from, Inject environment variables to the build process, Properties Content)
```
ORGS=["YOUR_GITHUB_USERNAME"]
REPOS=["docker-jenkins-job", "docker-jenkins"]
```
You can omit REPOS, if you would like it to generate jobs for all your repos.
Both variables contain json as a string.
Another usecase is to not enter another ORGS, and just filter out the REPOS
you're interested in and let your local jenkins build those.
