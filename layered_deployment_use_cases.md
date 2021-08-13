### Use cases

#### Templating

As a user I'd like to use a templated layered deployment manifest with the IoTEdgeDev tool to generate a valid layered deployment manifest

**Impact**: Internal changes to IoTEdgeDev only

**Functionality**:

- Generate config files from layered deployment manifest templates
- Replace template variables in the config

**Considerations:**

- Should IoTEdgeDev tool fail on invalid layered deployment templates, such as only system modules? Or should we leave that up to the azure cli command, in order not to bind ourselves to that?

#### Build

As a user I'd like to use a templated layered deployment manifest with the IoTEdgeDev tool to build custom docker images in the template

**Impact**: Internal changes to IoTEdgeDev only

**Functionality**:

- Build docker images in the layered deployment manifest

**Considerations:**

- Are the tests for the regular deployments enough to cover edge cases? (Such as building without pushing, ect)

#### Push

As a user I'd like to use a templated layered deployment manifest with the IoTEdgeDev tool to push custom docker images in the template to my ACR

**Impact**: Internal changes to IoTEdgeDev only

**Functionality**:

- Push docker images in the layered deployment manifest

**Considerations:**

- Are the tests for the regular deployments enough to cover edge cases? (Such as pushing without building, ect)

#### Deploy

As a user I'd like to use a layered deployment manifest with the IoTEdgeDev tool to deploy the layer to IoT Hub

**Impact**: Changes to all instances of the deploy command (`deploy`, `push --deploy`, `build --deploy`)
> Note this may be a good opportunity to split up the tasks, start with just changing `deploy`, for example

**Functionality**:

- Set layered deployment priority by command line
- Set layered deployment name by command line
- Set deployment `target-condition` or `target-condition` tag name (To be decided) by `.env` config
  - `"--target-condition", f"{target_tag_name}=true"` vs `"--target-condition", target_condition"`
  - Leaning towards full `target-condition`, but I believe there may have been an issue with
  - Full `target-condition` requires additional testing to verify what happens on an invalid target condition

**Considerations:**

- Maybe differentiate between base and layered deployments (right now the `az iot edge deployment` `--layered` flag seems to not make a difference to as to what the deployment is deployed as). May be possible to figure out if the layered deployment is a base or layered by looking at the manifest layout
- Could implement without changing the `deploy` command by using the `.env`, but that may lead to a poorer user experience, due to each deployment requiring a unique name and priority
- Priority of the base deployment must be lower than the layer deployments to ensure correct behavior of the layered deployments (For example, with layered deployments and base deployments of the same priority, if the base deployment is deployed after the layered, the layered wont be applied, but will be if the order is reverse. In addition a layered deployment  as a base deployment is unapplied if a base deployment with the same priority is deployed later)

#### Tag Edge Device

As a user I'd like to tag my edge device with the corresponding `target-condition` of my layered deployment

**Impact**: New command (suggestion: `tag-edge-device`)

**Functionality**:

- Add a tag to an IoT Edge(?) device
  - The tag should correspond to the deployment (maybe with a command line override to add any tag?)

**Considerations:**

- Is there any difference in tagging a IoT device vs an IoT Edge device?

#### Simulation

As a user I'd like to use my layered deployment manifest with the IoTEdgeDev tool to deploy to my simulated environment

**Impact**: Unknown

**Functionality**:

- Deploy containers to an simulated local environment using layered deployments

**Considerations:**

- Is this feasible?
