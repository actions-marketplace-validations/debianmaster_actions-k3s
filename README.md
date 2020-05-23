# Hello world JavaScript action

This github action starts a k3s cluster of choosen version and sets KUBECONFIG to created cluster.
This is helpful for testing applications /  operators on top of kubernetes cluster

## Inputs
version
| k3s                       | k8s      |
|---------------------------|----------|
| v1.18.2-k3s1              | v1.18.2  |
| v1.17.4-k3s1              | v1.17.4  |
| v1.17.3-k3s1              | v1.17.3  |
| v1.17.2-k3s1              | v1.17.2  |
| v1.17.0-k3s.1             | v1.17.0. |
| v1.16.9-k3s1              | v1.16.9  |
| v0.10.0, 0.10.1, 0.10.2   | v1.16.2. |
| v0.9.0, 0.9.1             | v1.15.4  |
| v0.8.1                    | v1.14.6. |
| v0.8.0                    | v1.14.5. |
| v0.7.0                    | v1.14.4  |
| 0.6.0, 0.6.1              | v1.14.3. |


## Outputs
* KUBECONFIG environment variable set to k3s cluster created as part of action.
* kubeconfig: Kubeconfig location 
## Example usage
```yaml
- uses: debianmaster/action-k3s@master
  id: k3s
  with:
    version: 'v0.9.1'
- run: |
    kubectl get nodes
    kubectl get pods -A
    sleep 20
    kubectl get pods -A
```
```yaml
name: k3s Testing
on: push
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
    - uses: debianmaster/action-k3s@master
      id: k3s
      with:
        version: 'v0.9.1'
    - run: |
        kubectl get nodes
```

```yaml
name: Matrix Testing
on: push
jobs:
  build:
    runs-on: ubuntu-latest
    strategy:
      matrix:
        k8s_version: [v0.8.1,v0.9.0,v1.16.9-k3s1,v1.17.4-k3s1,v1.18.2-k3s1]
    steps:
    - uses: debianmaster/action-k3s@master
      id: k3s
      with:
        version: ${{ matrix.k8s_version }}
    - name: Test on k3s
      run: |
        kubectl get nodes
```
