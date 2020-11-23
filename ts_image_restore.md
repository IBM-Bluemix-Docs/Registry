---

copyright:
  years: 2017, 2020
lastupdated: "2020-11-23"

keywords: troubleshooting, support, help, errors, problems, ts, registry, restoring images, restoring images from the trash, trash

subcollection: Registry

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:download: .download}
{:tsSymptoms: .tsSymptoms}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}
{:term: .term}
{:external: target="_blank" .external}
{:troubleshoot: data-hd-content-type='troubleshoot'}

# Why am I getting a 409 error telling me that the tagged image already exists when I'm restoring an image from the trash?
{: #troubleshoot-image-restore}
{: troubleshoot}
{: support}

You want to restore an image from the {{site.data.keyword.registrylong}} trash, but you get a 409 error: `The tagged image already exists. You can restore this image by using the digest.`
{: shortdesc}

{: tsSymptoms}
You receive the following error message when you run the [`ibmcloud cr image-restore`](/docs/Registry?topic=container-registry-cli-plugin-containerregcli#bx_cr_image_restore) command: `The tagged image already exists. You can restore this image by using the digest.`

{: tsCauses}
An image with the same name exists in your live repository. You can't overwrite a live image with an image that is in the trash.

{: tsResolve}
Untag the existing image in your live repository by running the [`ibmcloud cr image-untag`](/docs/Registry?topic=container-registry-cli-plugin-containerregcli#bx_cr_image_untag) command. You can then restore the required image from the trash by running the [`ibmcloud cr image-restore`](/docs/Registry?topic=container-registry-cli-plugin-containerregcli#bx_cr_image_restore) command with the option `<repo>@<digest>`, which restores the digest and its tags to the live repository. For more information, see [Restoring images by digest](/docs/Registry?topic=Registry-registry_images_#registry_images_restore_digest).