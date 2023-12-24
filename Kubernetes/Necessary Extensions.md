YAML extensions use করতে হবে in VSCode

<img width="500" alt="Screenshot 2023-12-22 at 9 11 37 PM" src="https://github.com/Mohsem35/CKA-Certification/assets/58659448/658e8cfc-b3d2-4da6-a463-4f69b465a5d9">


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
