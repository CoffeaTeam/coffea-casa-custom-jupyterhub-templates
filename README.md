# Custom JupyterHub UI Templates for Coffea-casa AF

Basic Jupyterhub UI customization for the Coffea-casa AF.

## Kubernetes Configuration

(based on [configuration instructions](https://discourse.jupyter.org/t/customizing-jupyterhub-on-kubernetes/1769/3))

Append the config below to your standard config to use the custom templates provided with this repo:

```python
jupyterhub:
  hub:
    # clone custom JupyterHub templates into a volume
    initContainers:
      - name: git-clone-templates
        image: alpine/git
        args:
          - clone
          - --single-branch
          - --branch=main
          - --depth=1
          - --
          - https://github.com/CoffeaTeam/coffea-casa-custom-jupyterhub-templates.git
          - /etc/jupyterhub/custom
        securityContext:
          runAsUser: 0
        volumeMounts:
          - name: custom-templates
            mountPath: /etc/jupyterhub/custom
    extraVolumes:
      - name: custom-templates
        emptyDir: {}
    extraVolumeMounts:
      - name: custom-templates
        mountPath: /etc/jupyterhub/custom

    extraConfig:
      templates: |
        c.JupyterHub.template_paths = ['/etc/jupyterhub/custom/templates']
```

