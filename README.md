# local_ollama_docker_sandbox_1
Just a learning project to spin up a local ML model using Ollama and Docker (and Hugging Face?)

Running a large language model locally with Ollama and Docker is an easy and private way to use AI. Here's how to do it.

-----

### Step 1: Install Required Software

You'll need to install the following software to run Ollama and Docker:

  * **Docker Desktop:** This is an application that helps you run, manage, and deploy applications inside containers.
  * **Visual Studio Code** (or any code editor): You will use this to create a configuration file.

-----

### Step 2: Set Up the Docker Container

1.  **Create a folder** for your project.
2.  **Open the folder** in your code editor and create a new file named `docker-compose.yml`.
3.  **Add the following code** to the file to define your Ollama service:

<!-- end list -->

```yaml
services:
  ollama:
    container_name: ollama
    image: ollama/ollama:latest
    ports:
      - 11434:11434
    volumes:
      - models:/root/.ollama
    restart: always

volumes:
  models:
```

  * `container_name`: Gives your container a simple name.
  * `image`: Specifies the Docker image to use. `ollama/ollama:latest` tells Docker to use the most recent version of Ollama.
  * `ports`: Exposes port `11434` from the container to the same port on your local machine, allowing you to access it.
  * `volumes`: Creates a dedicated storage location for your downloaded models so they aren't lost if the container is stopped or restarted.
  * `restart: always`: Ensures the container automatically restarts if it crashes.

-----

### Step 3: Run the Ollama Container

1.  **Open your terminal** and navigate to the folder where you saved the `docker-compose.yml` file.
2.  Run the following command to start the container: `docker compose up -d`
3.  Verify that the container is running by opening **Docker Desktop**. You should see a container named `ollama` in your list.

-----

### Step 4: Download a Model

1.  Return to your terminal.

2.  Use the following command to download a model inside your running Ollama container:
    `docker exec -it ollama ollama pull tinyllama`

      * `docker exec -it ollama`: Executes a command inside the container named `ollama`.
      * `ollama pull tinyllama`: The command that tells Ollama to download the `tinyllama` model.
      * **Note:** You can replace `tinyllama` with any model from the [Ollama models page](https://ollama.com/library).

-----

### Step 5: Test the Model with a Request

Once the model is downloaded, the Ollama service is ready to receive requests. You can use an API client like **Postman** to test it.

1.  Create a **new Post request**.

2.  Enter the URL: `http://localhost:11434/api/generate`

3.  Go to the **Body** tab, select **raw** and set the format to **JSON**.

4.  Enter the following JSON object, customizing the `model`, `prompt`, and other parameters as you wish:

    ```json
    {
      "model": "tinyllama",
      "prompt": "Give a short shopping list for a normal week. Include food, cleaning, and self-care. Only one or two items per category.",
      "stream": false,
      "options": {
        "max_tokens": 50,
        "temperature": 0.7
      }
    }
    ```

5.  **Send the request.** The model will process the prompt and return a response in the body of the reply.

By following these steps, you have successfully set up a private, local large language model that you can use for your personal projects. You can easily move this setup to a cloud server to deploy your own private AI API.
