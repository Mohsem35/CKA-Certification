



YAML extensions use করতে হবে in VSCode

For using this extension at K8s configuration file, we need to modfiy some settings 

To enable Kubernetes support


**1st approach:**

gear icon of `YAML` extension -> `Extensions Settings` -> scroll to bottom to find `Yaml:Schemas`-> click on `Edit in settings.json` -> paste following code -> restart vs code

```json
{
    "yaml.schemas": {     
      "kubernetes": "*.yaml"
    }
}
```

**2nd approach:**

crtl + p