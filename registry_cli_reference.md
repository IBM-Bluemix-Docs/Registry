---

copyright:
  years: 2017
lastupdated: "2017-10-09"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}


# {{site.data.keyword.registrylong_notm}} (`bx cr`) commands for managing Docker images in your namespace
{: #registry_cli_reference}

You can use the container-registry plug-in to set up your own image namespace in an IBM-hosted and managed private registry where you can securely store, and share Docker images with all users in your {{site.data.keyword.Bluemix}} account.
{:shortdesc}


## bx cr commands
{: #registry_cli_reference_bxcr}

Run bx cr commands in the {{site.data.keyword.registryshort}} CLI.
{:shortdesc}

For supported commands, see [{{site.data.keyword.registrylong_notm}} CLI](../../cli/plugins/registry/index.html#containerregcli).

## Formatting and filtering the CLI output for {{site.data.keyword.registrylong_notm}} commands
{: #registry_cli_listing}

You can format and filter the CLI output for supported {{site.data.keyword.registrylong_notm}} commands.
{:shortdesc}

By default, the CLI output is displayed in a human-readable format. However, this view might limit your ability to use the output, particularly if the command is run programmatically. For example, in the `bx cr image-list` CLI output, you might want to sort the `Size` field by numerical size, but the command returns a string description of the size. The container-registry plug-in provides the format option that you can use to apply a Go template to the CLI output. The Go template is a feature of the [Go programming language](https://golang.org/pkg/text/template/) that lets you customize the CLI output.

You can alter the CLI output by applying the format option in two different ways:

1.  Format data in your CLI output. For example, change the `Created` field output from Unix time to standard time.
2.  Filter data in your CLI output. For example, filter by details of the image to display a specific subset of images by using the Go template `if gt` condition.

You can use the format option with the following {{site.data.keyword.registrylong_notm}} commands. Click on a command to view a list of available fields and their data types.

-   [bx cr image-list](registry_cli_reference.html#registry_cli_listing_imagelist)
-   [bx cr image-inspect](registry_cli_reference.html#registry_cli_listing_imageinspect)
-   [bx cr token-list](registry_cli_reference.html#registry_cli_listing_tokenlist)

The following code examples demonstrate how you might use the formatting and filtering options.

-   Run the following command to display repository, tag, and vulnerability status of all images that have a size over 1 MB:

    ```
    bx cr image-list --format "{{ if gt .Size 1000000 }}{{ .Repository }}:{{ .Tag }} {{ .Vulnerable }}{{end}}"
    ```
    {: pre}

    Example output:

    ```
    example-registry.<region>.bluemix.net/user1/ibmliberty:latest OK
    example-registry.<region>.bluemix.net/user1/ibmnode:1 Vulnerable
    example-registry.<region>.bluemix.net/user1/ibmnode:test1 Vulnerable
    example-registry.<region>.bluemix.net/user1/ibmnode2:test2 Vulnerable
    ```
    {: screen}

-   Run the following command to display where IBM documentation is hosted for a specified IBM public image:

    ```
    bx cr image-inspect ibmliberty --format "{{ .ContainerConfig.Labels }}"

    ```
    {: pre}

    Example output:

    ```
    map[doc.url:/docs/images/docker_image_ibmliberty/ibmliberty_starter.html]
    ```
    {: screen}

-   Run the following command to display the exposed ports for a specified image:

    ```
    bx cr image-inspect ibmliberty --format "{{ .Config.ExposedPorts }}"

    ```
    {: pre}

    Example output:

    ```
    map[9080/tcp: 9443/tcp:]
    ```
    {: screen}

-   Run the following command to display all read-only tokens:

    ```
    bx cr token-list --format "{{ if eq .ReadOnly true}}{{.ID}} - {{.Expiry}} - {{.ReadOnly}} - {{.Description}}{{ end }}"
    ```
    {: pre}

    Example output:

    ```
    0a3fb35f-e8eb-5232-b9fb-b1bdcb36d68a - 1495798639 - true - demo
    ```
    {: screen}


### Go template options and data types in the `bx cr image-list` command
{: #registry_cli_listing_imagelist}

Review the following table to find available Go template options and data types for the `bx cr image-list` command.
{:shortdesc}

|Field|Type|Description|
|-----|----|-----------|
|`Created`|Integer (64 bit)|Displays when the image was created, expressed in number of seconds in [Unix time](https://en.wikipedia.org/wiki/Unix_time).|
|`Digest`|String|Displays the unique identifier for an image.|
|`Namespace`|String|Displays the namespace where the image is stored.|
|`Repository`|String|Displays the repository of the image.|
|`Size`|Integer (64 bit)|Displays the size of the image in bytes.|
|`Tag`|String|Displays the tag for the image.|
|`Vulnerable`|String|Displays the vulnerability status for the image. The possible statuses are described in [Managing image security with Vulnerability Advisor](../va/va_index.html).|
{: caption="Table 1. Available fields and data types in the bx cr image-list command." caption-side="top"}

### Go template options and data types in the `bx cr image-inspect` command
{: #registry_cli_listing_imageinspect}

Review the following table to find available Go template options and data types for the `bx cr image-inspect` command.
{:shortdesc}

|Field|Type|Description|
|-----|----|-----------|
|`ID`|String|Displays the unique identifier for an image.|
|`Parent`|String|Displays the ID of the parent image that was used to build this image.|
|`Comment`|String|Displays the description of the image.|
|`Created`|String|Displays the [Unix timestamp](https://en.wikipedia.org/wiki/Unix_time) when the image was created.|
|`Container`|String|Displays the ID of the container that created the image.|
|`ContainerConfig`|Object|Displays the default configuration for containers that are started from this image. See field details in [Config](registry_cli_reference.html#config).|
|`DockerVersion`|String|Displays the Docker version that was used to build this image.|
|`Author`|String|Displays the author of the image.|
|`Config`|Object|Displays configuration metadata for the image. See field details in [Config](registry_cli_reference.html#config).|
|`Architecture`|String|Displays the processor architecture that was used to build this image, and that is required to run the image.|
|`Os`|String|Displays the operating system family that was used to build this image, and that is required to run the image.|
|`OsVersion`|String|Displays the version of the operating system that was used to build this image.|
|`Size`|Integer (64 bit)|Displays the size of the image in bytes.|
|`VirtualSize`|Integer (64 bit)|Displays the sum of the size of each layer in the image in bytes.|
|`RootFS`|Object|Displays metadata that describe the root file system for the image. See field details in [RootFS](registry_cli_reference.html#rootfs).|
{: caption="Table 2. Available fields and data types in the bx cr image-inspect command." caption-side="top"}

#### Config

|Field|Type|Description|
|-----|----|-----------|
|`Hostname`|String|Displays the host name of the container.|
|`Domainname`|String|Displays the fully qualified domain name of the container.|
|`User`|String|Displays the user that runs commands inside the container where the image is used.|
|`AttachStdin`|Boolean|Displays _true_ if the standard input stream is attached to the container and _false_ if not.|
|`AttachStdout`|Boolean|Displays _true_ if the standard output stream is attached to the container and _false_ if not.|
|`AttachStderr`|Boolean|Displays _true_ if the standard error stream is attached to the container and _false_ if not.|
|`ExposedPorts`|Key-value map|Displays the list of exposed ports in the format `[123:,456:]`.|
|`Tty`|Boolean|Displays _true_ if a pseudo-tty is allocated to the container and _false_ if not.|
|`OpenStdin`|Boolean|Displays _true_ if the standard input stream is opened and _false_ if the standard input stream is closed.|
|`StdinOnce`|Boolean|Displays _true_ if the standard input stream is closed after the attached client disconnects and _false_ if the standard input stream stays open.|
|`Env`|Array of strings|Displays the list of environment variables in the form of key-value pairs.|
|`Cmd`|Array of strings|Describes the commands and arguments that are passed to a container to be executed when the container is started.|
|`Healthcheck`|Object|Describes how to check that the container is healthy. See field details in [Healthcheck](registry_cli_reference.html#healthcheck).|
|`ArgsEscaped`|Boolean|Displays true if the command is already escaped (Windows specific).|
|`Image`|String|Displays the name of the image that was passed by the operator.|
|`Volumes`|Key-Value map|Displays the list of volume mounts that are mounted to a container.|
|`WorkingDir`|String|Displays the working directory inside the container where specified commands are executed.|
|`Entrypoint`|Array of strings|Describes the command that is executed when the container starts.|
|`NetworkDisabled`|Boolean|Displays _true_ if the networking is disabled for the container and _false_ if the networking is enabled for the container.|
|`MacAddress`|String|Displays the MAC address that is assigned to the container.|
|`OnBuild`|Array of strings|Displays the ONBUILD metadata that were defined on the image Dockerfile.|
|`Labels`|Key-value map|Displays the list of labels that were added to the image as key-value pairs.|
|`StopSignal`|String|Describes the UNIX stop signal to send when to stop the container.|
|`StopTimeout`|Integer|Displays the timeout in seconds to stop a container.|
|`Shell`|Array of strings|Displays the shell-form of RUN, CMD, ENTRYPOINT.|
{: caption="Table. " caption-side="top"}

#### Healthcheck

|Field|Type|Description|
|-----|----|-----------|
|`Test`|Array of strings|Displays how to perform the health check test. Available options are:<ul><li>{}: inherit the health check</li><li>{"NONE"}: the health check is disabled</li><li>{"CMD", args...}: exec arguments directly</li><li>{"CMD-SHELL", command}: run the command with the system's default shell</li></ul>|
|`Interval`|Integer (64 bit)|Displays the time to wait between two health checks in nanoseconds.|
|`Timeout`|Integer (64 bit)|Displays the time to wait before considering the health check to have failed in nanoseconds.|
|`Retries`|Integer|Displays the number of consecutive failures that are needed to consider a container as unhealthy.|
{: caption="Table 4. Available fields and data types in the Healthcheck struct." caption-side="top"}

#### RootFS

|Option|Type|Description|
|------|----|-----------|
|`Type`|String|Displays the type of file system.|
|`Layers`|Array of strings|Displays the descriptors of each image layer.|
|`BaseLayer`|String|Displays the descriptor for the base layer in the image.|
{: caption="Table 5. Available fields and data types in the RootFS struct." caption-side="top"}

### Go template options and data types in the `bx cr token-list` command
{: #registry_cli_listing_tokenlist}

Review the following table to find available Go template options and data types for the `bx cr token-list` command.
{:shortdesc}

|Field|Type|Description|
|-----|----|-----------|
|`ID`|String|Displays the unique identifier for a token.|
|`Expiry`|Integer (64 bit)|Displays [Unix timestamp](https://en.wikipedia.org/wiki/Unix_time) when the token expires.|
|`ReadOnly`|Boolean|Displays _true_ when you can pull images only and _false_ when you can push and pull images to and from your namespace.|
|`Description`|String|Displays the description of the token.|
{: caption="Table 6. Available fields and data types in the bx cr token-list command." caption-side="top"}
