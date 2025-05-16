# intensivregister-branch-name
 
This GitHub Action computes the image name based on the current Git branch, pull request, or tag. It dynamically determines an appropriate image tag and outputs it for further use in workflows.

## Description
 
This action provides a standardized way to generate an image tag based on the following scenarios:
* **Pushes/Merges to the `master` branch**: The image tag will be set to the branch name.
* **Tags**: The image tag will be set to the tag name.
* **Pull Requests**: The image tag will be prefixed with PR- followed by the pull request number.

## Outputs
 
`image-name`: The computed name of the image.
* Description: Name of the image.
* Value: `${{ steps.image-name-outputter.outputs.image-name }}`

## Usage

Below is an example of how to use this action in a workflow:
```
on:
  push:
    tags:
      - 'v*'
    branches:
      - 'master'
  pull_request:
    branches:
      - 'master'

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
        ...

      - name: Get image name
        id: image-name
        uses: intensivregister/intensivregister-branch-name@v1.3

      - name: Build image and push to harbor
        uses: docker/build-push-action@v3
        with:
          push: true
          tags: <registry>/intensivregister/<project>:${{ steps.image-name.outputs.image-name }} 
```