{quiz, id: quiz_ch2}

? Why does enabling a GPU matter for this workshop?

a) DESeq2 requires a GPU.
B) The large language model (LLM) is hosted locally and runs much faster on a GPU.
c) It speeds up the cloud storage data transfer during the bucket copy process.
d) It is required by AnVIL on RStudio.

? You provision an RStudio instance with 8 CPUs, 52 GB of system memory, and 1 NVIDIA Tesla T4 GPU (16 GB VRAM). You plan to load a 19 GB model (e.g. qwen3-coder). What will happen when you load the model?

a) The model will fit entirely within the 16 GB VRAM of the Tesla T4 GPU.
b) The model will fail to load completely because it exceeds the total system memory of 52 GB.
C) The application will automatically split the model layers between the GPU and the CPU system memory, allowing it to run successfully despite being slower than a fully resident model.
d) The environment will automatically upgrade your hardware to a larger GPU to accommodate the full 19 GB model in VRAM.

? What is the primary purpose of running the `nohup` command?

A) To run the Ollama server in the background, keeping it alive even if the terminal session ends or the tab is closed, while redirecting output to a log file.
b) To automatically update the server to the latest version and pin the release for workshop reproducibility.
c) To force the 19 GB model download to run concurrently with the server startup.
d) To execute the server commands directly inside the R Console without throwing an error.

? If a user's network connection drops halfway through downloading the 19 GB qwen3-coder:30b model, what happens when they rerun the pull command?

a) The download fails permanently, requiring them to delete the ollama directory and start from scratch.
B) The ollama pull command is resumable, meaning the download picks up where it left off rather than starting over from 0%.
c) Ollama automatically falls back to downloading the smaller llama3.1 model instead.
d) The model gets corrupted, forcing the user to manually purge the cache using `ollama clean` before retrying.

? In the command `nohup ollama/bin/ollama serve`, what is the role of the `serve` argument passed to Ollama?

a) It forces termination of active background sessions.
B) It initializes and runs the local API server.
c) It compiles the local source code into an executable.
d) It downloads and installs the latest binary updates.

{/quiz}
