# Setting up self-hosted TC environment using Docker

## Download TC Server Image
`docker pull jetbrains/teamcity-server`

## Download TC Agent Image
`docker pull jetbrains/teamcity-agent`

## Create Empty Data and Log Dirs
Create a directories in your file structure for both the TC Docker image data and logs

## Run TC Server
```
docker run --name teamcity-server-instance  \
    -v <path-to-data-directory>:/data/teamcity_server/datadir \
    -v <path-to-logs-directory>:/opt/teamcity/logs  \
    -p <port-on-host>:8111 \
    jetbrains/teamcity-server
```

`<port-on-host>` => open port on your local machine

`docker ps` to confirm that container is running

## Navigate to TC Server in Browser

Open your browser and go to `localhost:<port-on-host>`

## Select your DB Type

Select internal default database for testing and demo purposes and follow the rest of the setup wizard

## Create Dir for TC Agent Data
Create a dir just like you did for the teamcity server

## Run the TC Agent Image

```
docker run --name teamcity-agent-instance -e SERVER_URL="<url to TeamCity server>"  \ 
    -v <path to agent config folder>:/data/teamcity_agent/conf  \      
    jetbrains/teamcity-agent
```

NOTE: the server URL cannot be `localhost` as this refers to localhost in the container itself

## Configure the TC Agent

- Login to the TC Server
- Navigate to the Agents tab
- Look under the `Unauthorized` tab, you should see your agent there if it has been setup correctly
- Click `Authorize`
- Select your agent pool and you're good to go

## Customising the Agent
- Use the minimal agent => `jetbrains/teamcity-minimal-agent`
- Login to the agent using:
	`docker exec -u 0 -it my-customized-agent bash`
- Commit your changes using:
	`docker commit [CONTAINER_ID] [new_image_name]`
	
- Make sure to update the env vars in `C:\teamcity\custom_agent`
