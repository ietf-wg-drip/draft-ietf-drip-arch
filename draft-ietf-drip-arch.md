---
title: Drone Remote Identification Protocol (DRIP) Architecture
abbrev: DRIP Architecture 
docname: draft-ietf-drip-arch-12
date: 2021-05-10

stand_alone: true

ipr: trust200902
area: ART
wg: drip
kw: Internet-Draft
cat: info

coding: utf-8
pi: [toc, sortrefs, symrefs]

author:
      -
        ins: S. Card
        name: Stuart W. Card
        org: AX Enterprize
        street: 4947 Commercial Drive
        city: Yorkville, NY
        code: 13495
        country: USA
        email: stu.card@axenterprize.com
      -
        ins: A. Wiethuechter
        name: Adam Wiethuechter
        org: AX Enterprize
        street: 4947 Commercial Drive
        city: Yorkville, NY
        code: 13495
        country: USA
        email: adam.wiethuechter@axenterprize.com
      -
        ins: R. Moskowitz
        name: Robert Moskowitz
        org: HTT Consulting
        street: ""
        city: Oak Park, MI
        code: 48237
        country: USA
        email: rgm@labs.htt-consult.com
      - 
        ins: S. Zhao (Editor)
        name: Shuai Zhao
        org: Tencent
        street: 2747 Park Blvd
        city: Palo Alto
        code: 94588
        country: USA
        email: shuai.zhao@ieee.org
      - 
        ins: A. Gurtov
        name: Andrei Gurtov
        org: Linköping University
        street: IDA
        city: Linköping
        code: SE-58183 Linköping
        country: Sweden
        email: gurtov@acm.org
normative:

  I-D.ietf-drip-reqs: drip-requirements
  RFC2119:
  RFC8174:
  

informative:
  RFC8002:
  RFC8032:
  RFC7482:
  F3411-19:
    title: Standard Specification for Remote ID and Tracking
    author:
      -
        org: ASTM
    date: 2019

  CTA2063A:
    title: Small Unmanned Aerial Systems Serial Numbers
    author:
      -
        org: ANSI
    date: 2019

  Delegated:
    title: EU Commission Delegated Regulation 2019/945 of 12 March 2019 on unmanned aircraft systems and on third-country operators of unmanned aircraft systems
    author:
      -
        org: European Union Aviation Safety Agency (EASA)
    date: 2019

  Implementing:
    title: EU Commission Implementing Regulation 2019/947 of 24 May 2019 on the rules and procedures for the operation of unmanned aircraft
    author: 
      -
        org: European Union Aviation Safety Agency (EASA)
    date: 2019
  LAANC: 
    title: Low Altitude Authorization and Notification Capability
    author: 
      -
        org: United States Federal Aviation Administration (FAA)
    target: https://www.faa.gov/uas/programs_partnerships/data_exchange/
  NPRM:
    title: Notice of Proposed Rule Making on Remote Identification of Unmanned Aircraft Systems
    author:
      -
        org: United States Federal Aviation Administration (FAA)
    date: 2019
  TS-22.825: 
    title: UAS RID requirement study
    author: 
      -
        org: 3GPP
    target: https://portal.3gpp.org/desktopmodules/Specifications/SpecificationDetails.aspx?specificationId=3527
  U-Space:
    title:  U-space Concept of Operations
    author:
      -
        org: European Organization for the Safety of Air Navigation (EUROCONTROL)
    target: https://www.sesarju.eu/sites/default/files/documents/u-space/CORUS%20ConOps%20vol2.pdf
    date: 2019
  
  FAA_RID:
    title: Remote Identification of Unmanned Aircraft
    author:
      -
        org: United States Federal Aviation Administration (FAA)
    target: https://www.govinfo.gov/content/pkg/FR-2021-01-15/pdf/2020-28948.pdf
    date: 2021

  FAA_UAS_Concept_Of_Ops:
    title: Unmanned Aircraft System (UAS) Traffic Management (UTM) Concept of Operations (V2.0)
    author:
      -
        org: United States Federal Aviation Administration (FAA)
    target: https://www.faa.gov/uas/research_development/traffic_management/media/UTM_ConOps_v2.pdf
    date: 2020
  
  RFC7033:
  RFC7401:
  RFC7484:
  RFC8004:
  RFC5731:
  RFC1034:
  RFC5730:
  RFC3972:
  RFC8949:

  I-D.ietf-drip-rid: drip-uas-rid

--- abstract

This document describes an architecture for protocols and services to
support Unmanned Aircraft System Remote Identification and tracking
(UAS RID), plus RID-related communications, conforming to proposed and final
regulations plus external technical standards, satisfying the
requirements listed in the companion requirements document
{{-drip-requirements}}.

--- middle

# Introduction #        {#introduction}

This document describes an architecture for protocols and services to
support Unmanned Aircraft System Remote Identification and tracking
(UAS RID), plus RID-related communications, conforming to proposed and final
regulations plus external technical standards, satisfying the
requirements listed in the companion requirements document
{{-drip-requirements}}.

This document assumes the reader is familiar with {{-drip-requirements}}. 

## Overview of Unmanned Aircraft System (UAS) Remote ID (RID) and Standardization ## 

UAS Remote Identification (RID) is an application enabler for a UAS to be identified by Unmanned Aircraft Systems Traffic Management (UTM) and UAS Service Supplier (USS) ({{appendix-a}}) or third parties entities such as law enforcement.  Many safety and other considerations dictate that UAS be remotely identifiable.  Civil Aviation Authorities (CAAs) worldwide are mandating UAS RID.  The European Union Aviation Safety Agency (EASA) has published {{Delegated}} and {{Implementing}} Regulations.  

CAAs currently promulgate performance-based regulations that
do not specify techniques, but rather cite industry consensus
technical standards as acceptable means of compliance.

Federal Aviation Administration (FAA)

> The FAA published a Notice of Proposed Rule Making
{{NPRM}} in 2019 and whereafter published the Final Rule {{FAA_RID}} in 2021. In FAA's final rule, it is clearly stating that Automatic Dependent Surveillance Broadcast (ADS-B) Out and transponders can not be used to serve the purpose of an remote identification. (More about ADS-B in {{adsb}})

 American Society for Testing and Materials (ASTM)

> ASTM International, Technical Committee F38 (UAS), Subcommittee F38.02 (Aircraft Operations), Work Item WK65041, developed the ASTM {{F3411-19}} Standard Specification for Remote ID and Tracking.

> ASTM defines one set of RID information and two means, MAC-layer
broadcast and IP-layer network, of communicating it.  If a UAS uses 
both communication methods, the same information must be
provided via both means.  The {{F3411-19}} is cited by FAA in its RID final rule
{{FAA_RID}} as "a potential means of compliance" to a Remote ID rule.


The 3rd Generation Partnership Project (3GPP)

> With release 16, 3GPP completed the UAS RID requirement study
{{TS-22.825}} and proposed use cases in the mobile network and the
services that can be offered based on RID.  Release 17
specification works on enhanced UAS service requirements and
provides the protocol and application architecture support which
is applicable for both 4G and 5G network.

## Overview of Types of UAS Remote ID ## 

### Broadcast RID ### {#brid}

A set of RID messages are defined for direct, one-way, broadcast
transmissions from the UA over Bluetooth or Wi-Fi.  These are currently defined as MAC-Layer messages. Internet (or other Wide Area Network) connectivity is only needed for UAS registry information lookup by Observers using the locally directly received UAS RID as a key.  Broadcast RID should be functionally usable in situations with no Internet connectivity.

The Broadcast RID is illustrated in {{brid-fig}} below.

~~~
               x x  UA
              xxxxx
                |
                |
                |     app messages directly over  
                |     one-way RF data link (no IP)
                |
                |
                +
                x
              xxxxx
                x
                x
                x x   Observer's device (e.g. smartphone)
              x   x

~~~
{: #brid-fig}

With Broadcast RID, an Observer is limited to their radio "visible"
airspace for UAS awareness and information.  With Internet queries using harvested
RID (see {{harvestbridforutm}}), the Observer may gain more information about those visible UAS.

### Network RID ### {#nrid}

A RID data dictionary and data flow for Network RID are defined in {{F3411-19}}.
This data flow is emitted from a UAS via unspecified means (but at least in part over the Internet)
to a Network Remote ID Service Provider (Net-RID SP).
These Net-RID SPs provide the RID data to Network Remote ID Display Providers (Net-RID DP). 
It is the Net-RID DP that responds to queries from Network Remote ID Observers  (expected typically, but not specified exclusively, to be web-based) specifying airspace
volumes of interest. Network RID depends upon connectivity, in several segments, 
via the Internet, from the UAS to the Observer.

The Network RID is illustrated in {{nrid-fig}} below:

~~~
            x x  UA
            xxxxx       ********************
             |   \    *                ------*---+------------+
             |    \   *              /       *  | NET_RID_SP |
             |     \  * ------------/    +---*--+------------+
             | RF   \ */                 |   *
             |        *      INTERNET    |   *  +------------+
             |       /*                  +---*--| NET_RID_DP |
             |      / *                  +---*--+------------+
             +     /   *                 |   *
              x   /     *****************|***      x
            xxxxx                        |       xxxxx
              x                          +-------  x
              x                                    x
             x x   Operator (GCS)      Observer   x x
            x   x                                x   x

~~~
{: #nrid-fig}

Command and Control (C2) must flow from the GCS to the UA via some path, currently (in the year of 2021) typically a direct RF link, but with increasing BVLOS operations expected often to be wireless links at either end with the Internet between. For all but the simplest hobby aircraft, telemetry (at least position and heading) flows from the UA to the GCS via some path, typically the reverse of the C2 path. Thus RID information pertaining to both the GCS and the UA can be sent, by whichever has Internet connectivity, to the Net-RID SP, typically the USS managing the UAS operation.

The Net-RID SP forwards RID information via the Internet to subscribed Net-RID DP, typically a USS. Subscribed Net-RID DP forward RID information via the Internet to subscribed Observer devices. Regulations require and {{F3411-19}} describes RID data elements that must be transported end-to-end from the UAS to the subscribed Observer devices.

{{F3411-19}} prescribes the protocols only between the  Net-RID SP, Net-RID DP, and the Discovery and Synchronization Service (DSS). DRIP may also address standardization of protocols between the UA and GCS, between the UAS and the Net-RID SP, and/or between the Net-RID DP and Observer devices.

>> Informative note: Neither link layer protocols nor the use of links (e.g., the link often existing between the GCS and the UA) for any purpose other than carriage of RID information is in the scope of {{F3411-19}} Network RID.

## Overview of USS Interoperability ## 

Each UAS is registered to at least one USS.  With Net-RID, there is
direct communication between the UAS and its USS.  With Broadcast-RID, the UAS Operator has either pre-filed a 4D space volume for USS operational knowledge and/or Observers can be providing information about observed UA to a USS.  USS exchange information via a Discovery and Synchronization Service (DSS) so all USS collectively have knowledge about all activities in a 4D airspace.

The interactions among Observer, UA, and USS are shown in {{inter-uss}}.

~~~
                            +----------+ 
                            | Observer |
                            +----------+
                           /            \
                          /              \      
                   +-----+                +-----+         
                   | UA1 |                | UA2 |
                   +-----+                +-----+       
                          \              /      
                           \            /                       
                            +----------+                            
                            | Internet | 
                            +----------+ 
                           /            \
                          /              \
                    +-------+           +-------+
                    | USS-1 | <-------> | USS-2 |   
                    +-------+           +-------+
                             \         /
                              \       /
                              +------+
                              |  DSS |
                              +------+
~~~
{: #inter-uss}


## Overview of DRIP Architecture ##

The requirements document {{-drip-requirements}} also provides an extended introduction to the problem space, use cases, etc.  Only a brief summary of that introduction will be restated here as context, with reference to the general UAS RID usage scenarios shown in {{arch-intro}} below.

~~~

      General      x                           x     Public
      Public     xxxxx                       xxxxx   Safety
      Observer     x                           x     Observer
                   x                           x
                  x x ---------+  +---------- x x
                 x   x         |  |          x   x
                               |  |
         UA1 x x               |  |  +------------ x x UA2
            xxxxx              |  |  |            xxxxx
               |               +  +  +              |
               |            xxxxxxxxxx              |
               |           x          x             |
               +----------+x Internet x+------------+
    UA1        |           x          x             |       UA1 
   Pilot     x |            xxxxxxxxxx              | x    Pilot
  Operator  xxxxx              + + +                xxxxx Operator
   GCS1      x                 | | |                  x    GCS2
             x                 | | |                  x
            x x                | | |                 x x
           x   x               | | |                x   x
                               | | |
             +----------+      | | |       +----------+
             |          |------+ | +-------|          |
             | Public   |        |         | Private  |
             | Registry |     +-----+      | Registry |
             |          |     | DNS |      |          |
             +----------+     +-----+      +----------+

~~~
{: #arch-intro}

DRIP will enable leveraging existing Internet resources (standard
protocols, services, infrastructure, and business models) to meet UAS
RID and closely related needs.  DRIP will specify how to apply IETF
standards, complementing {{F3411-19}} and other external standards, to
satisfy UAS RID requirements.  DRIP will update existing and develop
new protocol standards as needed to accomplish the foregoing.

This document will outline the UAS RID architecture into which DRIP
must fit and the architecture for DRIP itself.  This includes
presenting the gaps between the CAAs' Concepts of Operations and
{{F3411-19}} as it relates to the use of Internet technologies and UA
direct RF communications.  Issues include, but are not limited to:

> * Design of trustworthy remote ID and trust in RID messages ({{rid}})

> * Mechanisms to leverage Domain Name System (DNS: {{RFC1034}}), 
Extensible Provisioning Protocol (EPP {{RFC5731}}) and Registration Data Access Protocol (RDAP) ({{RFC7482}})
to provide for private ({{privateinforeg}}) and public ({{publicinforeg}}) Information Registry.

> * Harvesting broadcast remote ID messages for UTM inclusion ({{harvestbridforutm}})

> * Privacy in RID messages (PII protection) ({{privacyforbrid}})


# Conventions #  

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL
NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "NOT RECOMMENDED",
"MAY", and "OPTIONAL" in this document are to be interpreted as
described in BCP 14 {{RFC2119}} {{RFC8174}} when, and only when, they
appear in all capitals, as shown above.


# Definitions and Abbreviations # {#definitionsandabbr}

## Additional Definitions ##

This document uses terms defined in {{-drip-requirements}}.

## Abbreviations ## 

ADS-B:  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Automatic Dependent Surveillance Broadcast

DSS: &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Discovery & Synchronization Service

EdDSA: &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Edwards-Curve Digital Signature Algorithm

GCS: &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Ground Control Station

HHIT:&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Hierarchical HIT Registries

HIP: &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Host Identity Protocol

HIT: &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Host Identity Tag

RID: &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Remote ID

Net-RID SP: Network RID Service Provider

Net-RID DP: Network RID Display Provider.

PII: &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Personally Identifiable Information

RF: &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Radio Frequency

SDSP:&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Supplemental Data Service Provider

UA: &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Unmanned Aircraft

UAS: &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Unmanned Aircraft System

USS: &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;UAS Service Supplier

UTM: &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;UAS Traffic Management


## Claims, Assertions, Attestations, and Certificates ## 

This section introduces the terms "Claims", "Assertions", "Attestations", and "Certificates" as used in DRIP.

This is due to the term "certificate" having significant technological and legal baggage associated with it, specifically around X.509
certificates. These types of certificates and Public Key Infrastructure invoke more legal and public policy considerations
than probably any other electronic communication sector. It emerged as a governmental platform for trusted identity management and was
pursued in intergovernmental bodies with links into treaty instruments.

Claims:

> A claim in DRIP is a predicate (e.g., "X is Y", "X has property Y", and most importantly "X owns Y" or "X is owned by Y").  One basic use case of a claim is an entity using an HHIT as an identifier, e.g., a UAS using an HHIT as a UAS ID.

Assertions:

> An assertion in DRIP is a set of claims.  This definition is borrowed from JWT/CWT.  An HHIT of itself can be seen as an assertion: a claim that the identifier is a handle to an asymmetric keypair owned by the entity, and a claim that the identifier is in the registry specified by the HID embedded in the identifier.

Attestations:  

> An attestation in DRIP is a signed assertion.  The signer may be a claimant or a third party.  Under DRIP this is normally used when an entity asserts a relationship with another entity, along with other information, and the asserting entity signs the assertion, thereby making it an attestation. 

Certificates:

> A certificate in DRIP is an attestation, strictly over identity information, signed by a third party.


# HHIT for DRIP Entity Identifier # {#rid}

This section describes the basic requirements of a DRIP entity identifier per regulation constrains from ASTM {{F3411-19}} and explains the use of Hierarchical Host Identity Tags (HHITs) as self-asserting IPv6 addresses and thereby a trustable DRIP identifier for use as the UAS Remote ID.  HHITs self-attest to the included explicit hierarchy that provides Registrar discovery for 3rd-party ID attestation.

## UAS Remote Identifiers Problem Space ##

A DRIP entity identifier needs to be "Trustworthy". This means that within the framework of the RID messages, an Observer can establish that the DRIP identifier used does uniquely belong to the UAS.  That the only way for any other UAS to assert this DRIP identifier would be to steal something from within the UAS. The DRIP identifier is self-generated by the UAS (either UA or GCS) and registered with the USS.

The data communication of using Broadcast RID faces extreme challenges due to the limitation of the demanding support for Bluetooth. The ASTM {{F3411-19}} defines the basic RID message which is expected to contain certain RID data and the Authentication message. The Basic RID message has a maximum payload of 25 bytes and the maximum size allocated by ASTM for the RID is 20 bytes and only 3 bytes are left unused. currently, the authentication maximum payload is defined to be 201 bytes.

Standard approaches like X.509 and PKI will not fit these constraints, even using the new EdDSA {{RFC8032}} algorithm cannot fit within the maximum 201 byte limit, due in large measure to ASN.1 encoding format overhead.

An example of a technology that will fit within these limitations is an enhancement of the Host Identity Tag (HIT) of HIPv2 {{RFC7401}} using Hierarchical HITs (HHITs) for UAS RID is outlined in HHIT based UAS RID {{I-D.ietf-drip-rid}}. As PKI with X.509 is being used in other systems with which UAS RID must interoperate (e.g. Discovery and Synchronization Service and any other communications involving USS) mappings between the more flexible but larger X.509 certificates and the HHIT-based structures must be devised. This could be as in {{RFC8002}} or simply the HHIT as Subject Alternative Name (SAN) and no Distinguished Name (DN).

A self-attestation of the HHIT RID can be done in as little as 84 bytes, by avoiding an explicit encoding technology like ASN.1 or Concise Binary Object Representation (CBOR {{RFC8949}}). This compressed attestation consists of only the HHIT, a timestamp, and the EdDSA signature on them. The HHIT prefix and suiteID provide crypto agility and implicit encoding rules. Similarly, a self-attestation of the Hierarchical registration of the RID (an attestation of a RID third-party registration "certificate") can be done in 200 bytes.  Both these are detailed in UAS RID {{I-D.ietf-drip-rid}}.

An Observer would need Internet access to validate a self-attestations claim.  A third-party Certificate can be validated via a small credential cache in a disconnected environment.  This third-party Certificate is possible when the third-party also uses HHITs for its identity and the UA has the public key and the Certificate for that HHIT.

## HIT as A Trustworthy DRIP Entity Identifier ##

A Remote ID that can be trustworthily used in the RID Broadcast mode can be built from an asymmetric keypair. Rather than using a key signing operation to claim ownership of an ID that does not guarantee name uniqueness, in this method the ID is cryptographically derived directly from the public key. The proof of ID ownership (verifiable attestation, versus mere claim) comes from signing this cryptographic ID with the associated private key. It is statistically hard for another entity to create a public key that would generate (spoof) the ID.

HITs are so designed; they are statistically unique through the cryptographic hash feature of second-preimage resistance. The cryptographically-bound addition of the Hierarchy and an HHIT registration process (e.g. based on Extensible Provisioning Protocol, {{RFC5730}}) provide complete, global HHIT uniqueness. This registration forces the attacker to generate the same public key rather than a public key that generates the same HHIT. This is in contrast to general IDs (e.g. a UUID or device serial number) as the subject in an X.509 certificate.

## HHIT for DRIP Identifier Registration and Lookup ##
<!-- 
DRIP identifiers need a deterministic lookup mechanism that rapidly provides
actionable information about the identified UA.  The identifier itself needs to
be the inquiry input into the lookup given the constraints imposed by some of the
broadcast media.  This can best be achieved by an Identifier registration
hierarchy cryptographically embedded within the Identifier. -->

Remote ID needs a deterministic lookup mechanism that rapidly provides actionable information about the identified UA.  Given the size constraints imposed by the Bluetooth 4 broadcast media, the Remote ID itself needs to be the inquiry input into the lookup.  An HHIT DRIP identifier contains cryptographically embedded registration information.  This HHIT registration hierarchy, along with the IPv6 prefix, is trustable and sufficient information that can be used to perform such a lookup.  Additionally, the IPv6 prefix can enhance the HHITs use beyond the basic Remote ID function (e.g use in HIP, {{RFC7401}}).

<!-- 
A HHIT itself consists of a registration hierarchy, the hashing crypto suite information, and the hash of these items along with the underlying public key.  Additional information, e.g. an IPv6 prefix, can enhance the HHITs use beyond the basic Remote ID function (e.g use in HIP, {{RFC7401}}). 
 -->

Therefore, a DRIP identifier can be represented as a HHIT.  It can be self-generated by a UAS (either UA or GCS) and registered with the Private Information Registry (More details in {{privateinforeg}}) identified in its hierarchy fields. Each DRIP identifier represented as an HHIT can not be used more than once.

A DRIP identifier can be assigned to a UAS as a static HHIT by its manufacturer, such as a single HI and derived HHIT encoded as a hardware serial number per {{CTA2063A}}.  Such a static HHIT can only be used to bind one-time use DRIP identifiers to the unique UA.  Depending upon implementation, this may leave a HI private key in the possession of the manufacturer (more details in  {{sc}}).

In another case, a UAS equipped for Broadcast RID can be provisioned not only with its HHIT but also with the HI public key from which the HHIT was derived and the corresponding private key, to enable message signature.  A UAS equipped for Network RID can be provisioned likewise; the private key resides only in the ultimate source of Network RID messages (i.e. on the UA itself if the GCS is merely relaying rather than sourcing Network RID messages).  Each Observer device can be provisioned either with public keys of the DRIP identifier root registries or certificates for subordinate registries.

HHITs can be used throughout the UAS/UTM system. The Operators, Private Information Registries, as well as other UTM entities, can use HHITs for their IDs. Such HHITs can facilitate DRIP security functions such as used with HIP to strongly mutually authenticate and encrypt communications.

## HHIT for DRIP Identifier Cryptographic ## 

The only (known to the authors of this document at the time of its writing) extant fixed-length ID cryptographically derived from a public key are the Host Identity Tag {{RFC7401}}, HITs, and Cryptographically Generated Addresses {{RFC3972}}, CGAs. However, both HITs and CGAs lack registration/retrieval capability. HHIT, on the other hand, is capable of providing a cryptographic hashing function, along with a registration process to mitigate the probability of a hash collision (first registered, first allowed).  

# DRIP Identifier Registration and Registries # {#ei}

UAS registries can hold both public and private UAS information resulting from the DRIP identifier registration process.  Given these different uses, and to improve scalability, security, and simplicity of administration, the public and private information can be stored in
different registries. A DRIP identifier is amenable to handling as an Internet domain name (at an arbitrary level in the hierarchy). It also can be registered in at least a pseudo-domain (e.g. .ip6.arpa for reverse lookup), or as a sub-domain (for forward lookup). This section introduces the public and private information registries for DRIP identifiers. 


## Public Information Registry ## {#publicinforeg}

### Background ###

The public registry provides trustable information such as attestations of RID ownership and HDA registration.  Optionally, pointers to the repositories for the HDA and RAA implicit in the RID can be included (e.g. for HDA and RAA HHIT|HI used in attestation signing
operations).  This public information will be principally used by Observers of Broadcast RID messages.  Data on UAS that only use Network RID, is only available via an Observer's Net-RID DP that would tend to provide all public registry information directly.  The Observer can visually "see" these UAS, but they are silent to the Observer; the Net-RID DP is the only source of information based on a query for an airspace volume. 

### Proposed Approach ###

A DRIP public information registry can respond to standard DNS queries, in the definitive public Internet DNS hierarchy.  If a DRIP public information registry lists, in a HIP RR, any HIP RVS servers for a given DRIP identifier, those RVS servers can restrict relay services per AAA policy; this requires extensions to {{RFC8004}}.  These public information registries can use secure DNS transport (e.g. DNS over TLS) to deliver public information that is not inherently trustable (e.g. everything other than attestations).


## Private Information Registry ## {#privateinforeg}

### Background ###

The private information required for DRIP identifiers is similar to that required for Internet domain name registration.  A DRIP identifier solution can leverage existing Internet resources: registration protocols, infrastructure and business models, by fitting into an ID structure compatible with DNS names.  This implies some sort of hierarchy, for scalability, and management of this hierarchy.  It is expected that the private registry function will be provided by the same organizations that run USS, and likely integrated with USS.

### Proposed Approach ### 

A DRIP private information registry can support essential Internet domain name registry operations (e.g. add, delete, update, query) using interoperable open standard protocols.  It can also support the Extensible Provisioning Protocol (EPP) and the Registry Data Access
Protocol (RDAP) with access controls.  It might be listed in a DNS: that DNS could be private; but absent any compelling reasons for use of private DNS, a public DNS hierarchy needs to be in place. The DRIP private information registry in which a given UAS is registered needs to be findable, starting from the UAS ID, using the methods specified in {{RFC7484}}.  A DRIP private information registry can also support WebFinger as specified in {{RFC7033}}.

# Harvesting Broadcast Remote ID messages for UTM Inclusion # {#harvestbridforutm}

ASTM anticipated that regulators would require both Broadcast RID and
Network RID for large UAS, but allow RID requirements for small UAS
to be satisfied with the operator's choice of either Broadcast RID or
Network RID.  The EASA initially specified Broadcast RID for UAS of
essentially all UAS and is now also considering Network RID.  The FAA
RID Final Rules only specifies Broadcast RID for UAS, however, still encourages Network RID for complementary functionality, especially in support of UTM.

One obvious opportunity is to enhance the
architecture with gateways from Broadcast RID to Network RID. This
provides the best of both and gives regulators and operators
flexibility.  It offers considerable enhancement over some Network RID
options such as only reporting planned 4D operation space by the
operator.

These gateways could be pre-positioned (e.g. around
airports, public gatherings, and other sensitive areas) and/or
crowd-sourced (as nothing
more than a smartphone with a suitable app is needed).  As Broadcast
RID media have limited range, gateways receiving messages claiming
locations far from the gateway can alert authorities or a SDSP to the
failed sanity check possibly indicating intent to deceive.
Surveillance SDSPs can use messages with precise date/time/position
stamps from the gateways to multilaterate UA location, independent of
the locations claimed in the messages (which may have a natural time lag
as it is), which are entirely operator self-reported in UAS RID and UTM.  

Further, gateways with additional sensors (e.g. smartphones with cameras) can provide independent information on the UA type and size, confirming or refuting those claims made in the RID messages.  This Crowd Sourced Remote ID
(CS-RID) would be a significant enhancement, beyond baseline DRIP
functionality; if implemented, it adds two more entity types.

## The CS-RID Finder ##
A CS-RID Finder is the gateway for Broadcast Remote ID Messages into the UTM.  It performs this gateway function via a CS-RID SDSP.  A CS-RID Finder could implement, integrate, or accept outputs from, a Broadcast RID receiver.  However, it can not interface directly with a GCS, Net-RID SP, Net-RID DP or Network RID client.  It would present a TBD interface to a CS-RID SDSP; this interface needs to be based upon but readily distinguishable from that between a GCS and a Net-RID SP.

## The CS-RID SDSP ##

A CS-RID SDSP would appear (i.e. present the same interface) to a Net-RID SP as a Net-RID DP. A CS-RID SDSP can not present a standard GCS-facing interface as if it were a Net-RID SP. A CS-RID SDSP would present a TBD interface to a CS-RID Finder; this interface can be based upon but readily distinguishable between a GCS and a Net-RID SP.

# Privacy for Broadcast PII # {#privacyforbrid}

Broadcast RID messages can contain PII.  A viable architecture for PII protection would be symmetric
encryption of the PII using a key known to the UAS and its USS.
An authorized Observer could send the encrypted PII along with the
UAS ID (to entities such as USS of the Observer, or to the UAS in which the UAS ID is registered if that can be determined from the UAS ID itself or to a Public Safety USS) to get the plaintext.
Alternatively, the authorized Observer can receive the key to
directly decrypt all future PII content from the UA.

PII can be protected unless the UAS is informed otherwise.  This could
come from operational instructions to even permit flying in a space/time.
It can be special instructions at the start or during an operation.
PII protection can not be used if the UAS loses connectivity to
the USS.  The UAS always has the option to abort the operation if PII
protection is disallowed.

An authorized Observer can instruct a UAS via the USS that conditions
have changed mandating no PII protection or land the UA (abort the
operation).

# Security Considerations # {#sc}

The security provided by asymmetric
cryptographic techniques depends upon protection of the private keys.
A manufacturer that embeds a private key in an UA may have retained a
copy.  A manufacturer whose UA are configured by a closed source
application on the GCS which communicates over the Internet with the
factory may be sending a copy of a UA or GCS self-generated key back
to the factory.  Keys may be extracted from a GCS or UA. The RID
sender of a small harmless UA (or the entire UA) could be carried by
a larger dangerous UA as a "false flag."  Compromise of a registry
private key could do widespread harm.  Key revocation procedures are
as yet to be determined.  These risks are in addition to those
involving Operator key management practices.

# Acknowledgements #

The work of the FAA's UAS Identification and Tracking (UAS ID)
Aviation Rulemaking Committee (ARC) is the foundation of later ASTM
and proposed IETF DRIP WG efforts.  The work of ASTM F38.02 in
balancing the interests of diverse stakeholders is essential to the
necessary rapid and widespread deployment of UAS RID.  IETF
volunteers who have contributed to this draft include Amelia
Andersdotter and Mohamed Boucadair.


--- back

# Overview of Unmanned Aircraft Systems (UAS) Traffic Management (UTM) # {#appendix-a}

## Operation Concept ##

The National Aeronautics and Space Administration (NASA) and FAAs'
effort of integrating UAS's operation into the national airspace
system (NAS) leads to the development of the concept of UTM and the
ecosystem around it.  The UTM concept was initially presented in
2013 and version 2.0 is published in 2020 {{FAA_UAS_Concept_Of_Ops}}. 

The eventual development and implementation are conducted by
the UTM research transition team which is the joint workforce by FAA
and NASA.  World efforts took place afterward.  The Single European
Sky ATM Research (SESAR) started the CORUS project to research its
UTM counterpart concept, namely {{U-Space}}.  This effort is led by the
European Organization for the Safety of Air Navigation (Eurocontrol).

Both NASA and SESAR have published the UTM concept of operations to
guide the development of their future air traffic management (ATM)
system and make sure safe and efficient integrations of manned and
unmanned aircraft into the national airspace.

The UTM composes of UAS operation infrastructure, procedures and
local regulation compliance policies to guarantee UAS's safe
integration and operation.  The main functionality of a UTM includes,
but is not limited to, providing means of communication between UAS
operators and service providers and a platform to facilitate
communication among UAS service providers.

## UAS Service Supplier (USS) ##

A USS plays an important role to fulfill the key performance
indicators (KPIs) that a UTM has to offer.  Such Entity acts as a
proxy between UAS operators and UTM service providers.  It provides
services like real-time UAS traffic monitor and planning,
aeronautical data archiving, airspace and violation control,
interacting with other third-party control entities, etc.  A USS can
coexist with other USS(s) to build a large service coverage map which
can load-balance, relay and share UAS traffic information.

The FAA works with UAS industry shareholders and promotes the Low
Altitude Authorization and Notification Capability {{LAANC}} program
which is the first system to realize some of the UTM envisioned functionality.
The LAANC program can automate the UAS's flight plan application and
approval process for airspace authorization in real-time by checking
against multiple aeronautical databases such as airspace
classification and fly rules associated with it, FAA UAS facility
map, special use airspace, Notice to Airman (NOTAM), and Temporary
Flight Rule (TFR).

## UTM Use Cases for UAS Operations ## 

This section illustrates a couple of use case scenarios where UAS participation in UTM has significant safety improvement.

1. For a UAS participating in UTM and takeoff or land in a
controlled airspace (e.g., Class Bravo, Charlie, Delta and Echo
in United States), the USS where UAS is currently communicating
with is responsible for UAS's registration, authenticating the
UAS's fly plan by checking against designated UAS fly map
database, obtaining the air traffic control (ATC) authorization
and monitor the UAS fly path in order to maintain safe boundary
and follow the pre-authorized route.

1. For a UAS participating in UTM and take off or land in an
uncontrolled airspace (ex.  Class Golf in the United States),
pre-fly authorization must be obtained from a USS when operating
beyond-visual-of-sight (BVLOS) operation.  The USS either accepts
or rejects received intended fly plan from the UAS.  Accepted UAS
operation may share its current fly data such as GPS position and
altitude to USS.  The USS may keep the UAS operation status near
real-time and may keep it as a record for overall airspace air
traffic monitor.


## Automatic Dependent Surveillance Broadcast (ADS-B)  ## {#adsb}

The ADS-B is the de jure technology used in manned aviation for sharing location information, from the aircraft to ground and satellite-based systems, designed in the early 2000s. Broadcast RID is 
conceptually similar to ADS-B, but with the receiver target being the general public on generally available devices (e.g. smartphones).

For numerous technical reasons, ADS-B itself is not suitable for 
low-flying small UA. Technical reasons include but not limited to the following:

1. Lack of support for the 1090 MHz ADS-B channel on any consumer handheld devices
1. Weight and cost of ADS-B transponders on CSWaP constrained UA
1. Limited bandwidth of both uplink and downlink, which would likely be saturated by large numbers of UAS, endangering manned aviation

Understanding these technical shortcomings, regulators worldwide have ruled out the use of ADS-B for the small UAS for which UAS RID and DRIP are intended.



