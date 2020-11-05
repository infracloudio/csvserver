# The csvserver assignment

## Prerequisites
  - Read Docker orientation and setup: https://docs.docker.com/get-started/
  - Read Docker build and run your image: https://docs.docker.com/get-started/part2/
  - Read Get started with Docker Compose: https://docs.docker.com/compose/gettingstarted/
  - Read Prometheus getting started: https://prometheus.io/docs/prometheus/latest/getting_started/
  - Read Prometheus installation with Docker: https://prometheus.io/docs/prometheus/latest/installation/
  - Install Docker and docker-compose on your machine and run following commands,
    ```sh
    docker pull infracloudio/csvserver:latest
    docker pull prom/prometheus:v2.22.0
    ```
  - Fork this repository to your own GitHub acccount and clone it.

> **NOTE**: If you have a Windows machine, you can try to do this assignment on [WSL-2](https://docs.docker.com/docker-for-windows/wsl/) or use https://labs.play-with-docker.com 

## Part I
  1. Run the container image `infracloudio/csvserver:latest` in background and check if it's running.
  2. If it's failing then try to find the reason, once you find the reason, move to the next step.
  3. Write a bash script `gencsv.sh` to generate a file named `inputFile` whose content looks like:
     ```csv
     0, 234
     1, 98
     ```
     These are comma separated values with index and a random number.
     - The generated file should have 10 such entries.
     - Make sure that the generated file is readable by other users.
  4. Run the container again in the background with file from (3) available inside the container (remember the reason you found in (2)).
  5. Get shell access to the container and find the port on which the application is listening. Once done, stop / delete the running container.
  6. Same as (4), run the container and make sure,
     - The application is accessible on the host at http://localhost:9393
     - Set the environment variable `CSVSERVER_BORDER` to have value `Orange`.

The application should be accessible at http://localhost:9393, it should have the 10 entries from `inuptFile` and the welcome note should have an orange color border.

> **NOTE**: If you are using play-with-docker.com then you will see the number 9393 highlighted at the top. You can access the application by clicking on that instead of using http://localhost:9393

> **NOTE**: On play-with-docker.com, you can create files in the terminal and edit them with their online editor.

### Save the solution
  - Create a directory named `solution` your clone.
  - Create a file called `README.md` in the solution with all the commands you executed as part of this exercise.
  - Save the `gencsv.sh`, `inputFile` in the same directory as well.
  - Commit and push the changes to your fork.

## Part II
  0. Delete any containers running from the last part.
  1. Create a `docker-compose.yaml` file for the setup from part I.
  2. One should be able to run the application with `docker-compose up`.

### Save the solution
  - Copy the `docker-compose.yaml` to the `solution` directory.
  - Commit and push the changes to your fork.

## Part III
  0. Delete any containers running from the last part.
  1. Add Prometheus container (`prom/prometheus:v2.22.0`) to the docker-compose.yaml form part II.
  2. Configure Prometheus to collect data from our application at `<application>:<port>/metrics` endpoint. (Where the `<port>` is the port from I.5)
  3. Make sure that Prometheus is accessible at http://localhost:9090 on the host.
  4. Type `csvserver_records` in the query box of Prometheus. Click on Execute and then switch to the Graph tab.

The Prometheus instance should be accessible at http://localhost:9090, and it should show a straight line graph with value 10 (consider shrinking the time range to 5m).

### Save the solution
  - Update the `docker-compose.yaml` from the `solution` directory.
  - Add any other files you may have created to the `solution` directory.
  - Commit and push the changes to your fork.

## Possible errors / caveats on different host OS
  1. SELinux enabled GNU/Linux machine: `open /****/****: permission denied`
  ```
  2020/10/29 13:22:56 error while reading the file "/****/****": open /****/****: permission denied
  ```
  Check the permission of the file `inputFile` on host. If SELinux is enabled on the host then the argument to `-v` should be something like `****/inputFile:/****/****:z` (the extra `:z` at the end). Same thing needs to be in the docker-compose.yaml.

## Submitting the solution
Once you have pushed your progress,

- Make your fork private.
- Add `anju-infracloud` and `rahul-infracloud` as owners to the repository.
- Reply to the email with link to your fork.
