# Running LLaMA 2 locally on a system and integrating it for HRI

### By - Chandran N 

Running LLMs locally could potentially offer many benefits such as lower latency, higher data security and freedom from continued reliance on unpredictable factors such as internet connection, API call request failure etc.

This serves as a guide to run LLaMA 2 locally on a system though the performance will vary based on the specifications of the local computer. Ideally having a dedicated graphics card can improve speed and enable passing larger prompts to teach the model how to respond.

For running LLaMA we will not be using the popular LLaMA.cpp but rather a newer and faster approach called PowerInfer. You can find more details here - https://github.com/SJTU-IPADS/PowerInfer

You can follow the installation instructions directly from the GitHub link provided above but to maintain homogenity of instruction all the steps necessary have been listed here as well.

Firstly, clone the repository and install all necessary dependencies - 

    git clone https://github.com/SJTU-IPADS/PowerInfer
    cd PowerInfer
    pip install -r requirements.txt

 Once that is done, make your repo based on your system specs as follows - 
 
 -   If you have an NVIDIA GPU:

cmake -S . -B build -DLLAMA_CUBLAS=ON
cmake --build build --config Release

-   If you have just CPU:

cmake -S . -B build
cmake --build build --config Release

Next, you need to download the GGUF file of the respective model. This is usually a heavier download and will take some time. 

You can find the GGUF file of the 7B model here - [PowerInfer/ReluLLaMA-7B-PowerInfer-GGUF](https://huggingface.co/PowerInfer/ReluLLaMA-7B-PowerInfer-GGUF)

Once HuggingFace opens, navigate to the Files and Versions tab above next to model card. You will find a button on the left of Train from where you can clone the model as well. Alternatively you can manually download the files directly as well by clicking the download button next to the files. Don't forget to download the Activation library as well.

Furthermore, you can compress the model to a smaller size for faster inference at the cost of accuracy by performing quantization - 

    ./build/bin/quantize /ReluLLaMA-7B-PowerInfer-GGUF/llama-7b-relu.powerinfer.gguf /ReluLLaMA-7B-PowerInfer-GGUF/llama-7b-relu.q4.powerinfer.gguf  Q4_0 

After this, clone the chat-with-albert.txt from this repository and paste it inside the prompts directory of PowerInfer.

You can now run the model using -

    ./build/bin/main -m /PATH/TO/MODEL -n $output_token_count --repeat_penalty 1.0 --color -i -r "User:" -f ./prompts/chat-with-albert.txt

chat-with-albert.txt has all the prompts of the *albert_language_interaction* repository of github. So once the above line is executed, you should be able to interact with the chatbot directly and locally :)
