# beingtomgreen.ollama

A simple Ansible role to deploy [Ollama in a Docker container](https://github.com/ollama/ollama/blob/main/docs/docker.md).

Please note that I've not yet tested this with an NVIDIA GPU.

## Requirements

- Docker and Docker Compose installed on target hosts
- Ansible 2.15+
- Community.docker collection

## Role Variables

See [`defaults/main.yml`](defaults/main.yml) and [`vars/main.yml`](vars/main.yml) for more details.

### GPU Support

[Ollama's GPU support](https://github.com/ollama/ollama/blob/main/docs/gpu.md) documentation.

#### None/CPU (default)

To run on CPU only:

```yaml
ollama_gpu_type: "none"
```

#### AMD

To use an AMD GPU:

```yaml
ollama_gpu_type: "amd"
```

This will automatically set the `ollama_image_tag` variable to `rocm`, and will mount the appropriate dri

#### NVIDIA

To use an NVIDIA GPU:

```yaml
ollama_gpu_type: "nvidia"
```

If you're using an NVIDIA GPU you'll need the [NVIDIA Container Toolkit](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/install-guide.html#installation) installed.

You will also need to configure Docker to use the NVIDIA run time.

```bash
sudo nvidia-ctk runtime configure --runtime=docker
sudo systemctl restart docker
```

### Pulling Ollama Models

```yml
ollama_pull_models:
  - name: "gemma3"
    tag: "4b"
  - name: "deepseek-r1"
    tag: "70b"
```

### Deleting Ollama Models

```yml
ollama_delete_models:
  - name: "gemma3"
    tag: "4b"
  - name: "deepseek-r1"
    tag: "70b"
```

## Dependencies

- community.docker

Example Playbook
----------------

```yaml
- hosts: target_host
  vars:
    ollama_gpu_type: "amd"
    ollama_pull_models:
      - name: "gemma3"
        tag: "4b"
      - name: "deepseek-r1"
        tag: "70b"
  roles:
    - beingtomgreen.ollama
```

## License

[MIT](LICENSE)

## Author Information

[Tom Green](https://github.com/BeingTomGreen)
