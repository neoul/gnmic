### Description
With `path` command it is possible to generate and search through the XPATH style paths extracted from a YANG file.

!!! warning
    Experimental feature. Only supports Nokia conf and state models in combined variant[^1].

By extracting the XPATH styled paths from a YANG model it is made possible to utilize CLI search tools like `awk`, `sed` and alike to find the paths satisfying specific matching rules.

The embedded search capability allows to perform a quick and simple search through the model's paths using simple inclusion/exclusion operators.

### Flags
#### file
With the mandatory `--file` flag user specifies the path to a YANG file to extract paths from.

#### module
The `[-m | --module]` flag specifies the module name that is contained in the referenced YANG file. Defaults to `nokia-state`; to extract configuration paths the `nokia-conf` module name should be provided.

#### types
When `--types` flag is present the extracted paths will also have a corresponding type printed out.

#### path-type
The `--path-type` flag governs which style is used to display the path information. The default value is `xpath` which will produce the XPATH compatible paths.

The other option is `gnmi` which will result in the paths to be formatted using the gNMI Path Conventions.

=== "XPATH"
    ```
    /state/sfm[sfm-slot=*]/hardware-data/firmware-revision-status
    ```

=== "gNMI"
    ```
    elem:{name:"state"}  elem:{name:"sfm"  key:{key:"sfm-slot"  value:"*"}}  elem:{name:"hardware-data"}  elem:{name:"firmware-revision-status"}
    ```

#### search
With the `--search` flag present an interactive CLI search dialog is displayed that allows to navigate through the paths list and perform a search.

```
❯ gnmic path --file _test/nokia-state-combined.yang --search
Use the arrow keys to navigate: ↓ ↑ → ←  and : toggles search
? select path: 
    /state/aaa/radius/statistics/coa/dropped/bad-authentication
    /state/aaa/radius/statistics/coa/dropped/missing-auth-policy
  ▸ /state/aaa/radius/statistics/coa/dropped/invalid
    /state/aaa/radius/statistics/coa/dropped/missing-resource
    /state/aaa/radius/statistics/coa/received
    /state/aaa/radius/statistics/coa/accepted
    /state/aaa/radius/statistics/coa/rejected
    /state/aaa/radius/statistics/disconnect-messages/dropped/bad-authentication
    /state/aaa/radius/statistics/disconnect-messages/dropped/missing-auth-policy
↓   /state/aaa/radius/statistics/disconnect-messages/dropped/invalid
```

### Examples

```bash
# output to stdout the XPATH styled paths
# from the nokia-state module of nokia-state-combined.yang file
gnmic path --file nokia-state-combined.yang

# from the nokia-conf module
gnmic path -m nokia-conf --file nokia-conf-combined.yang

# with the gNMI styled paths
gnmic path --file nokia-state-combined.yang --path-type gnmi

# with path types
gnmic path --file nokia-state-combined.yang --types

# entering the interactive navigation prompt
gnmic path --file nokia-state-combined.yang --search
```
<script id="asciicast-319579" src="https://asciinema.org/a/319579.js" async></script>

[^1]: Nokia combined models can be found in [nokia/7x50_YangModels](https://github.com/nokia/7x50_YangModels/tree/master/latest_sros_20.5/nokia-combined) repo.