# iascable-starting-point

This is just a starting point to use IasCable.

Resource:

* [Iascable](https://github.com/cloud-native-toolkit/iascable)


### Step 1: Install CLI

```sh
curl -sL https://raw.githubusercontent.com/cloud-native-toolkit/iascable/main/install.sh | sh
```

### Step 2: Verify the installation

```sh
iascable build
```

* Example output:

```sh
Configure (and optionally deploy) the iteration zero assets

Options:
      --version                   Show version number   [boolean]
      --help                      Show help             [boolean]
  -u, --catalogUrl                The url of the module catalog.
                                  Can be https:// or file:/
                                  protocol.
   [default: "https://modules.cloudnativetoolkit.dev/index.yaml"]
  -i, --input                     The path to the bill of
                                  materials to use as input
                                                          [array]
  -r, --reference                 The reference BOM to use for
                                  the build               [array]
  -o, --outDir                    The base directory where the
                                  command output will be written
                                            [default: "./output"]
      --platform                  Filter for the platform
                                  (kubernetes or ocp4)
      --provider                  Filter for the provider (ibm or
                                  k8s)
      --tileLabel                 The label for the tile.
                                  Required if you want to
                                  generate the tile metadata.
      --name                      The name used to override the
                                  module name in the bill of
                                  material.               [array]
      --tileDescription           The description of the tile.
      --flattenOutput, --flatten  Flatten the generated output
                                  into a single directory (i.e.
                                  remove the terraform folder).
                                                        [boolean]
      --debug                     Flag to turn on more detailed
                                  output message        [boolean]
```

### Step 3: Create a `Bill of Materials`(BOM) file 

```sh
nano firstbom.yaml
```

Copy following content into the new file:

```yaml
apiVersion: cloudnativetoolkit.dev/v1alpha1
kind: BillOfMaterial
metadata:
  name: ibm-vpc
spec:
  modules:
    - name: ibm-vpc
    - name: ibm-vpc-subnets
```

### Step 4: Execute following command

```sh
iascable build -i firstbom.yaml
```

### Step 5: Verify the created content

```sh
├── firstbom.yaml
└── output
    ├── ibm-vpc
    │   ├── apply.sh
    │   ├── bom.yaml
    │   ├── dependencies.dot
    │   ├── destroy.sh
    │   └── terraform
    │       ├── docs
    │       │   ├── ibm-resource-group.md
    │       │   ├── ibm-vpc-subnets.md
    │       │   └── ibm-vpc.md
    │       ├── ibm-vpc.auto.tfvars
    │       ├── main.tf
    │       ├── providers.tf
    │       ├── variables.tf
    │       └── version.tf
    └── launch.sh
```

* Terraform

| Filename | Content |
| --- | --- |
| `main.tf` | Here you see a number of modules defined including the defined `ibm-vpc` and `ibm-vpc-subnets` from the BOM file. |
| `providers.tf` | Simply contains the needed cloud provider information. In that case what we need to specify for `IBM Cloud`. |
| `variables.ft` | Contains the specification for the used variable in the `main.tf` or other Terrafrom files. |
| `version.ft` | Contains the specification for the used Terrafrom provider sources and versions. In that case only IBM is listed.|