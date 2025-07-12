# beingtomgreen.ollama

A simple Ansible role to deploy [Ollama in a Docker container](https://github.com/ollama/ollama/blob/main/docs/docker.md).

Please note that I've not yet tested this with an NVIDIA GPU.

## Installation

Given that Galaxy seems to have abandoned roles, I suggest referencing this repository directly in your projects `requirements.yml`:

```yml
---

roles:
  - name: ollama
    src: https://github.com/BeingTomGreen/ansible-role-ollama.git

collections: []
```

You can then install the requirements as normal:

```bash
ansible-galaxy install -r requirements.yml
```

## Requirements

- [Docker](https://docs.docker.com/) and [Docker Compose](https://docs.docker.com/compose/) installed on target hosts

## Role Variables

See [`defaults/main.yml`](defaults/main.yml) and [`vars/main.yml`](vars/main.yml) for more details.

### GPU Support

More information on [Ollama's GPU support](https://github.com/ollama/ollama/blob/main/docs/gpu.md) in their documentation.

#### None/CPU (default)

To run models on the CPU:

```yaml
ollama_gpu_type: "none"
```

#### AMD

To run models on an AMD GPU:

```yaml
ollama_gpu_type: "amd"
```

This will automatically set the `ollama_image_tag` variable to `rocm`, and will mount the appropriate devices for AMD GPUs.

#### NVIDIA

To run models on an NVIDIA GPU:

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

- [community.docker](https://docs.ansible.com/ansible/latest/collections/community/docker/index.html)

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
