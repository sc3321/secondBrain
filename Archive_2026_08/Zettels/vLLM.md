Inference serving engine.

Main job is to load model, manage GPU memory, schedule requests, batch-work and expose an interface for requests to be served.

pyTorch/HF model weights -> vLLM or SGLang -> CUDA/Any runtime -> GPU -> Generated tokens back to client.

[[GPU]]
[[CUDA Model]]
[[SGLang]]
