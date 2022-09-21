---

copyright:
  years: 2017, 2022
lastupdated: "2022-09-21"

keywords: Key Protect integration, integrate COS with Key Protect, encrypt COS bucket

subcollection: key-protect

---

{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:external: target="_blank" .external}
{:codeblock: .codeblock}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:term: .term}

# Integrating with {{site.data.keyword.cos_full_notm}}
{: #integrate-cos}

{{site.data.keyword.keymanagementservicefull}} and
{{site.data.keyword.cos_full}} work together to help you own the security of
your at rest data. Learn how to add advanced encryption to your
{{site.data.keyword.cos_full}} resources by using the
{{site.data.keyword.keymanagementservicelong_notm}} service.
{: shortdesc}

## About {{site.data.keyword.cos_full_notm}}
{: #cos}

{{site.data.keyword.cos_full_notm}} provides cloud storage for unstructured
data. Unstructured data refers to files, audio/visual media, PDFs, compressed
data archives, backup images, application artifacts, business documents, or any
other binary object.

To maintain data integrity and availability, {{site.data.keyword.cos_full_notm}}
slices and disperses data to storage nodes across multiple geographic locations.
No complete copy of the data resides in any single storage node, and only a
subset of nodes needs to be available so you can fully retrieve the data on the
network.

Provider-side encryption is provided, so your data is secured at rest and in
flight. To manage storage, you create buckets and import objects with the
{{site.data.keyword.cloud_notm}} console, or programmatically by using the
[{{site.data.keyword.cos_full_notm}} REST API](/docs/cloud-object-storage?topic=cloud-object-storage-compatibility-api){: external}.

For more information, see
[About COS](/docs/cloud-object-storage?topic=cloud-object-storage-about-cloud-object-storage#about-cloud-object-storage){: external}.

## How the integration works
{: #kp-cos-how}

{{site.data.keyword.keymanagementserviceshort}} integrates with
{{site.data.keyword.cos_full_notm}} to help you achieve full control of the
security of your data.

As you move data into your instance of {{site.data.keyword.cos_full_notm}}, the
service automatically encrypts your objects with data encryption keys (DEKs).

Within {{site.data.keyword.cos_full_notm}}, DEKs are stored in the service
securely, near the resources that they encrypt. When you need to access a
bucket, the service checks your user permissions and decrypts the objects within
the bucket for you. This encryption model is called
_provider-managed encryption_.

To enable the security benefits of _customer-managed encryption_, you can add
envelope encryption to your DEKs in {{site.data.keyword.cos_full_notm}} by
integrating with the {{site.data.keyword.keymanagementserviceshort}} service.
With {{site.data.keyword.keymanagementserviceshort}}, you provision highly
secure root keys, which serve as a master keys that you control in the service.

When you create a bucket in {{site.data.keyword.cos_full_notm}}, you can
configure envelope encryption for the bucket at its creation. This added
protection wraps (or encrypts) the DEKs associated with the bucket by using a
root key that you manage in {{site.data.keyword.keymanagementserviceshort}}.

The practice, called _key wrapping_, uses multiple AES algorithms to protect the
privacy and the integrity of your DEKs, so only you control access to their
associated data.

Figure 1 shows how {{site.data.keyword.keymanagementserviceshort}}
integrates with {{site.data.keyword.cos_full_notm}} to further secure your
encryption keys.

![The figure shows a contextual view of envelope encryption.](../images/kp-cos-envelope.svg){: caption="Figure 1. Contextual view of envelope encryption." caption-side="bottom"}

To learn more about how envelope encryption works in
{{site.data.keyword.keymanagementserviceshort}}, see
[Protecting data with envelope encryption](/docs/key-protect?topic=key-protect-envelope-encryption).

## Adding envelope encryption to your storage buckets
{: #kp-cos-envelope}

[After you designate a root key in {{site.data.keyword.keymanagementserviceshort}}](/docs/key-protect?topic=key-protect-create-root-keys)
and
[grant access between your services](/docs/key-protect?topic=key-protect-integrate-services#grant-access),
you can enable envelope encryption for a specified storage bucket by using the
{{site.data.keyword.cos_full_notm}} GUI.

To enable advanced configuration options for your storage bucket, ensure that an
[authorization](/docs/key-protect?topic=key-protect-integrate-services#grant-access)
exists between your {{site.data.keyword.cos_full_notm}} and
{{site.data.keyword.keymanagementserviceshort}} instances.
{: tip}

To add envelope encryption to your storage bucket:

1. From your {{site.data.keyword.cos_full_notm}} dashboard, click
    **Create bucket**.

2. Specify the bucket's details.

3. In the **Advanced Configuration** section, select
    **Add {{site.data.keyword.keymanagementserviceshort}} Keys**.

4. From the list of {{site.data.keyword.keymanagementserviceshort}} service
    instances, select the instance that contains the root key that you want to
    use for key wrapping.

5. For **Key Name**, select the alias of the root key.

6. Click **Create** to confirm the bucket creation.

From the {{site.data.keyword.cos_full_notm}} GUI, you can browse the buckets
that are protected by a {{site.data.keyword.keymanagementserviceshort}} root
key.

## What's next
{: #cos-integration-next-steps}

- For more information about associating your storage buckets with
{{site.data.keyword.keymanagementserviceshort}} keys, see
[Manage encryption](/docs/cloud-object-storage?topic=cloud-object-storage-encryption#encryption){: external}.


