# Quiet-STaR

Code for [Quiet-STaR: Language Models Can Teach Themselves to Think Before Speaking](https://arxiv.org/abs/2403.09629).

This project is implemented by simply patching the base Mistral implementation in Huggingface `transformers` using a new `modeling_mistral.py` and a new `configuration_mistral.py` and otherwise applying standard `transformers` features (e.g. the default Trainer). Our patches were applied to Huggingface's `transformers` version `4.37.0.dev0` under `src/transformers/models/mistral/` -- we cannot guarantee that other changes to their implementation will not affect our implementation, so for reproducibility, we encourage using the same version.

One pitfall to be wary of: the model is not taught not to generate start and end thought tokens. Thus, when performing actual inference, it is necessary to mask these out.

We make an 8-thought-token ahead (including start and end tokens) model [available via Huggingface](https://huggingface.co/ezelikman/quietstar-8-ahead).


Getting started on sfcompute

wget https://bootstrap.pypa.io/get-pip.py
python3 get-pip.py
pip install jupyter numpy==1.26.2 torch==2.1.2 accelerate==0.25.0 datasets==2.14.6 tokenizers==0.15.0 huggingface-hub==0.19.4 safetensors==0.4.1 wandb==0.15.12 sentencepiece==0.1.99 git+https://github.com/huggingface/transformers@e737446
git clone https://github.com/jakob-bjorner/quiet-star.git
rm /usr/local/lib/python3.10/dist-packages/transformers/models/mistral/configuration_mistral.py /usr/local/lib/python3.10/dist-packages/transformers/models/mistral/modeling_mistral.py 
ln -s /root/quiet-star/configuration_mistral.py /usr/local/lib/python3.10/dist-packages/transformers/models/mistral/configuration_mistral.py 
ln -s /root/quiet-star/modeling_mistral.py /usr/local/lib/python3.10/dist-packages/transformers/models/mistral/modeling_mistral.py 
huggingface-cli login
wandb login
tmux
python3 quiet-star-train
