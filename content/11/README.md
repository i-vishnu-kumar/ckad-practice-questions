
*_Set Configuration Context_*

`kubectl config use-context dk8s`

##### (Question)

Task

Please complete the following:

- create web1 pods using `_web1.yaml_` in the current directory

- restrict traffic to pod `_db_` allows traffic from only pods with labels `_tier: db and tier: web_`

<details>
<summary>
Solution - Click to expand!
</summary>

```yaml

#Alias k=kubectl
alias k=kubectl

# Apply the file
k apply -f web1.yaml

# db-ingress-web.yaml

apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: db-ingress-web
spec:
  podSelector:
    matchLabels:
      tier: db # This is taken from the pod description, nut in the question, we have to allow traffic from pods with both the labels, so two podSelectors are defined
  policyTypes:
  - Ingress
  ingress:
    - from:
      - podSelector:
          matchLabels:
            tier: web
      - podSelector:
          matchLabels:
            tier: db  #As in question the request can come from another pod with the same label
      ports: # Can also work without declaring the ports for CKAD
        - port: 3306
          protocol: TCP

k apply -f db-ingress-web.yaml
  
```

</details>
