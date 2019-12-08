# Load Testing with Locust

In order to use Locust to load test the Bassa API, install the latest version of [Docker with Docker Compose or Docker Desktop](https://www.docker.com/).

## Automated Method (Linux and macOS only)
1. Move to the **core** directory (./Bassa/components/core) as follows:
```bash
cd components/core
```
2. Run the automated bash script **test-locust.sh** as follows:
```bash
./test-locust.sh
```
3. Refer to [Verifying Bassa](#verifying-bassa)


## Manual Method
In the below examples, a macOS machine is used, but these commands should be platform agnostic (assuming you follow the correct manual guides below should you elect not to use Docker).

### Starting a production Bassa instance

You can start up a production Bassa instance using Docker by running the following command in **./bassa**:

```bash
docker-compose up --build
```

You can also refer to the manual installation guides for [Windows](https://github.com/scorelab/Bassa/wiki/Windows-Installation-Guide), [macOS](https://github.com/scorelab/Bassa/wiki/MacOS-Installation-Guide), or [Linux](https://github.com/scorelab/Bassa/wiki/MacOS-Installation-Guide) if you wish to not use Docker.

### Installing Locust
While the container is building, you can install Locust by running (for Python3):

```bash
python3 -m pip install locustio
```

## Verifying Bassa

After you have installed Locust, wait for the container to start. Upon finishing, your terminal should resemble the following (you can verify if the services have successfully started by executing the command below):

![Screenshot](https://user-images.githubusercontent.com/29003194/70386124-8c07ce80-1963-11ea-8ed0-526b0fbb3e95.png)

You can verify that the various Bassa services are running by executing:

```bash
docker ps
```

![Screen Shot 2019-12-07 at 11.42.30 PM](https://user-images.githubusercontent.com/29003194/70386127-8e6a2880-1963-11ea-970b-ab6913227fda.png)

You should see all four of the above services.

#### Starting Locust

We can now navigate to **./bassa/components/core/tests** in our terminal and run Locust by executing the following in that directory:

```bash
locust
```

With this, Locust will start a web app that we can typically access at **localhost:8089** or **127.0.0.1:8089**.

We will then be presented with the following options:

![Screenshot](https://user-images.githubusercontent.com/29003194/70384421-20663700-194c-11ea-9b01-1af444f36f3e.png)

#### Locust Fields

For the **"Number of users to simulate"** field, specify any number representing the number of simulated users that you would like to interact with the API (for example, 1000). 

In **"Hatch rate"**, specify any number representing the speed upon which you want these simulated users to spawn (for example, 50).

For **"Host"**, in most cases, you may be able to specify **http://localhost:5000** or **http://127.0.0.1:5000**. 

#### Troubleshooting Hosts

If these addresses do not work, you may have to find the IP address of the `api` container by running the following command:

```bash
docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' REPLACE_CONTAINER_ID_HERE
```

replacing `REPLACE_CONTAINER_ID_HERE` with the `CONTAINER ID` of the `api` container shown by executing `docker ps`.

This will yield an IP address followed by a port to which you can prepend "http://" and use that as your host.

#### Viewing Test Data

Upon pressing the "Start Swarming" button, the tests will begin. You should see a dashboard that looks like the following:

![Screenshot](https://user-images.githubusercontent.com/29003194/70384493-345e6880-194d-11ea-8073-ce9ae5a46bd0.png)

You can also view the request rate and failure rate in a graphical form by viewing the **Charts** tab:

![Screenshot](https://user-images.githubusercontent.com/29003194/70384507-5ce66280-194d-11ea-8dcf-116196c4add1.png)
