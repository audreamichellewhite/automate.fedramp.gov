---
title: Contents
weight: 103
---
# FedRAMP SSP Contents

For SSP-specific content, each main section of the SSP is represented in this section, along with OSCAL code snippets for representing the information in OSCAL syntax. There is also XPath syntax for querying the code in an OSCAL-based FedRAMP SSP represented in XML format.

Content that is common across OSCAL file types is described in the *[FedRAMP OSCAL Documentation](/documentation).* This includes the following:

- [**Title Page**](/documentation/general-concepts/4-expressing-common-fedramp-template-elements-in-oscal/#title-page)
- [**Prepared By/For**](/documentation/general-concepts/4-expressing-common-fedramp-template-elements-in-oscal/#prepared-by-third-party)
- [**Record of Template Changes**](/documentation/general-concepts/oscal-file-concepts/#oscal-syntax-versions)
- [**Revision History**](/documentation/general-concepts/4-expressing-common-fedramp-template-elements-in-oscal/#document-revision-history)
- [**How to Contact Us**](/documentation/general-concepts/4-expressing-common-fedramp-template-elements-in-oscal/#how-to-contact-us)
- [**Document Approvers**](/documentation/general-concepts/4-expressing-common-fedramp-template-elements-in-oscal/#document-approvals)
- [**Acronyms and Glossary**](/documentation/general-concepts/4-expressing-common-fedramp-template-elements-in-oscal/#fedramp-standard-attachments-acronyms-lawsregulations)
- [**Laws, Regulations, Standards and Guidance**](/documentation/general-concepts/4-expressing-common-fedramp-template-elements-in-oscal/#additional-laws-regulations-standards-or-guidance)
- [**Attachments and Citations**](/documentation/general-concepts/4-expressing-common-fedramp-template-elements-in-oscal/#attachments-and-embedded-content)

It is not necessary to represent the following sections of the SSP template in OSCAL; however, tools should present users with this content where it is appropriate:

-   Any blue-text instructions found in the SSP template where the instructions are related to the content itself

-   Table of contents

-   Introductory and instructive content in section 1, such as references to NIST SP 800-60, Guide to Mapping Types and the definitions from FIPS Pub 199

-   The control origination definitions are in appendix A of the SSP template; however, please note that hybrid and shared are represented in OSCAL by specifying more than one control origination.

The OSCAL syntax in this documentation may be used to represent the High, Moderate, Low and LI-SaaS FedRAMP SSP Templates. Simply ensure the correct FedRAMP baseline is referenced using the `import-profile` statement.

**NOTE: The FedRAMP SSP template screenshots in the sections that follow vary slightly from the most current version of the FedRAMP rev 5 SSP template.**


---
## System Information

### Cloud Service Provider (CSP) Name

The cloud service provider (CSP) must be provided as one of the party assemblies within the metadata.

{{< figure src="/img/ssp-figure-4.png" title="FedRAMP SSP template CSP Name" alt="Screenshot of the CSP name in the FedRAMP SSP template." >}}

#### OSCAL Representation
{{< highlight xml "linenos=table, hl_lines=5" >}}
<system-security-plan>
    <metadata>
        <!-- CSP Name -->
        <party uuid=”uuid-of-csp” type=”organization”>
            <name>Cloud Service Provider (CSP) Name</name>
        </party>
    </metadata>
</system-security-plan>
{{</ highlight >}}

#### XPath Queries
{{< highlight xml "linenos=table" >}}
Cloud Service Provider (CSP) Name:
    /*/metadata/party[@uuid='uuid-of-csp']/name
{{</ highlight >}}

---
### System Name, Abbreviation, and FedRAMP Unique Identifier

The remainder of the system information is provided in the
system-characteristics assembly.

The FedRAMP-assigned application number is the unique ID for a FedRAMP system. OSCAL supports several system identifiers, which may be assigned by different organizations.

For this reason, OSCAL requires the identifier-type flag be present and have a value that uniquely identifies the issuing organization. FedRAMP requires its value to be "https://fedramp.gov" for all FedRAMP-issued application numbers.

{{< figure src="/img/ssp-figure-5.png" title="FedRAMP SSP template System Name and Package ID" alt="Screenshot of the system name, and package ID in the FedRAMP SSP template." >}}

This assembly defines the full name of the system and its short name. A FedRAMP OSCAL SSP must define the system name and its short name.

#### OSCAL Representation
{{< highlight xml "linenos=table, hl_lines=9-13" >}}
<system-security-plan>
    <metadata>
        <!-- CSP Name -->
        <party uuid="uuid-of-csp" type="organization">
            <name>Cloud Service Provider (CSP) Name</name>
        </party>
    </metadata>
    <system-characteristics>
        <!-- System Name & Abbreviation -->
        <system-name>System's Full Name</system-name>
        <system-name-short>System's Short Name or Acronym</system-name-short>        
        <!-- FedRAMP Unique Identifier -->
        <system-id identifier-type="http://fedramp.gov">F00000000</system-id>        
        <!--  cut -->        
    </system-characteristics>
    <!--  cut -->
</system-security-plan>
{{</ highlight >}}

<br />
{{<callout>}}

**FedRAMP Allowed Value** 

Required Identifier Type:
- identifier-type="https://fedramp.gov"

{{</callout>}}

#### XPath Queries
{{< highlight xml "linenos=table" >}}
    Information System Name:
        /*/system-characteristics/system-name
    Information System Abbreviation:
        /*/system-characteristics/system-name-short
    FedRAMP Unique Identifier:
        /*/system-characteristics/system-id[@identifier-type="https://fedramp.gov"]
{{</ highlight >}}

---

# Users

A FedRAMP System Security Plan (SSP) must document in its description of the system implementation the different kinds of users that interact with the system. For the list of kinds of users, each one must define its respective type, role(s), privilege(s), and sensitivity level for how they interact with the system.

## OSCAL Representation

{{< highlight xml "linenos=table" >}}
<system-security-plan>
    <system-implementation>
        <user uuid="44444444-0000-4000-9000-000000000004">
            <title>System Administrator</title>
            <prop name="type" value="internal"/>
            <prop name="privilege-level" value="read-write"/>
            <prop ns="https://fedramp.gov/ns/oscal" name="sensitivity" value="high-risk"/>
            <role-id>system-admin</role-id>
        </user>
    </system-implementation>
</system-security-plan>
{{</ highlight >}}

## Required Properties

Each user definition must include the following properties:

1. **User Type** (`type`):
   - Specifies whether the user is internal or external to the organization
   - Values: `internal`, `external`

2. **Privilege Level** (`privilege-level`):
   - Defines the user's access rights
   - Values: `read`, `read-write`, `write`, `full-access`

3. **Role ID** (`role-id`):
   - References a defined role in the system
   - Example: `system-admin`, `end-user`, `security-auditor`

4. **Sensitivity Level** (`sensitivity`):
   - Indicates the risk level associated with the user's access
   - Values: `high-risk`, `moderate-risk`, `low-risk`

## XPath Queries

To retrieve user information:

{{< highlight xml "linenos=table" >}}
User Title:
    /*/system-implementation/user/title

User Type:
    /*/system-implementation/user/prop[@name="type"]/@value

Privilege Level:
    /*/system-implementation/user/prop[@name="privilege-level"]/@value

Role ID:
    /*/system-implementation/user/role-id

Sensitivity Level:
    /*/system-implementation/user/prop[@ns='https://fedramp.gov/ns/oscal'][@name="sensitivity"]/@value
{{</ highlight >}}

## Best Practices

1. Always assign a unique UUID to each user definition.
2. Include clear, descriptive titles for each user type.
3. Ensure all required properties are specified.
4. Use consistent naming conventions for role-ids.
5. Document any custom properties in the system's metadata.

### Service Model

The core-OSCAL system-characteristics assembly has a property for the cloud service model.

{{< figure src="/img/ssp-figure-6.png" title="FedRAMP SSP template cloud service model" alt="Screenshot of the cloud service model in the FedRAMP SSP template." >}}

#### OSCAL Representation
{{< highlight xml "linenos=table, hl_lines=14-19" >}}
<system-security-plan>
    <metadata>
        <!-- CSP Name -->
        <party uuid="uuid-of-csp" type="organization">
            <name>Cloud Service Provider (CSP) Name</name>
        </party>
    </metadata>
    <system-characteristics>
        <!-- System Name & Abbreviation -->
        <system-name>System's Full Name</system-name>
        <system-name-short>System's Short Name or Acronym</system-name-short>        
        <!-- FedRAMP Unique Identifier -->
        <system-id identifier-type="http://fedramp.gov">F00000000</system-id>
        <!-- Service Model -->
        <prop name="cloud-service-model" value="saas">
            <remarks>
                <p>Remarks are required if service model is "other". Optional otherwise.</p>
            </remarks>
        </prop>

        <!--  cut -->        
    </system-characteristics>
    <!--  cut -->     
</system-security-plan>
{{</ highlight >}}

<br />
{{<callout>}}

**OSCAL Allowed Values** 

Valid Service Model values:
- saas
- paas
- iaas
- other

{{</callout>}}

#### XPath Queries
{{< highlight xml "linenos=table" >}}
    Service Model:
        /*/system-characteristics/prop[@name="cloud-service-model"]/@value
    Remarks on System's Service Model:
        /*/system-characteristics/prop[@name="cloud-service-model"]/remarks/node()
{{</ highlight >}}

**NOTE:**

-   A cloud service provider may define two or more cloud service models for the cloud service offering defined in the system security plan if applicable for customer use (IaaS and PaaS; IaaS and PaaS and SaaS; PaaS and SaaS). Cloud service providers may use a "cloud-service-model" prop for each applicable cloud service model.
-   If the service model is "other", the remarks field is required. Otherwise, it is optional.

---
### Deployment Model

The core-OSCAL system-characteristics assembly has a property for the cloud deployment model.

{{< figure src="/img/ssp-figure-7.png" title="FedRAMP SSP template cloud deployment model" alt="Screenshot of the cloud deployment model in the FedRAMP SSP template." >}}

#### OSCAL Representation
{{< highlight xml "linenos=table, hl_lines=20-25" >}}
<system-security-plan>
    <metadata>
        <!-- CSP Name -->
        <party uuid="uuid-of-csp" type="organization">
            <name>Cloud Service Provider (CSP) Name</name>
        </party>
    </metadata>
    <system-characteristics>
        <!-- System Name & Abbreviation -->
        <system-name>System's Full Name</system-name>
        <system-name-short>System's Short Name or Acronym</system-name-short>        
        <!-- FedRAMP Unique Identifier -->
        <system-id identifier-type="http://fedramp.gov">F00000000</system-id>
        <!-- Service Model -->
        <prop name="cloud-service-model" value="saas">
            <remarks>
                <p>Remarks are required if service model is "other". Optional otherwise.</p>
            </remarks>
        </prop>
        <!-- Deployment Model -->
        <prop name="cloud-deployment-model" value="public-cloud">
            <remarks>
                <p>Remarks are required if deployment model is "hybrid". Optional otherwise.</p>
            </remarks>
        </prop>      
        <!--  cut -->        
    </system-characteristics>
    <!--  cut -->     
</system-security-plan>
{{</ highlight >}}

<br />
{{<callout>}}

**FedRAMP Accepted Values**
- name="cloud-deployment-model"

    Valid: public-cloud, private-cloud, government-only-cloud, hybrid-cloud, other

{{</callout>}}

#### XPath Queries
{{< highlight xml "linenos=table" >}}
    Deployment Model:
        /*/system-characteristics/prop[@name="cloud-deployment-model"]/@value
    Remarks on System's Deployment Model:
        /*/system-characteristics/prop[@name="cloud-deployment-model"]/remarks/node()
{{</ highlight >}}

**NOTE:**

-   A cloud service provider may define one and only one cloud deployment model in the system security plan for a cloud service offering.

-   OSCAL 1.0.0 permits a cloud-deployment-model of value community-cloud, but FedRAMP does not permit such a deployment model for cloud service offerings and is not permitted for a FedRAMP OSCAL-based system security plan.
-   If the deployment model is \"hybrid\", the remarks field is required. Otherwise, it is optional.

---
### Digital Identity Level (DIL) Determination

The digital identity level identified in the FedRAMP SSP template document, illustrated in the figure below, isexpressed through the following core OSCAL properties.

{{< figure src="/img/ssp-figure-8.png" title="FedRAMP SSP template DIL determination." alt="Screenshot of the DIL determination in the FedRAMP SSP template." >}}

#### OSCAL Representation
{{< highlight xml "linenos=table, hl_lines=14-17" >}}
<system-security-plan>
    <metadata>
        <!-- cut CSP Name -->
    </metadata>
    <system-characteristics>
        <!-- System Name & Abbreviation -->
        <system-name>System's Full Name</system-name>
        <system-name-short>System's Short Name or Acronym</system-name-short>        
        <!-- FedRAMP Unique Identifier -->
        <system-id identifier-type="http://fedramp.gov">F00000000</system-id>
        <!-- cut Service Model -->
        <!-- cut Deployment Model -->

        <!-- DIL Determination -->
        <prop name="identity-assurance-level" value="1"/>
        <prop name="authenticator-assurance-level" value="1"/>
        <prop name="federation-assurance-level" value="1"/>  
              
        <!--  cut -->        
    </system-characteristics>
    <!--  cut -->     
</system-security-plan>
{{</ highlight >}}

<br />
{{<callout>}}

**OSCAL Allowed Values**

Valid IAL, AAL, and FAL values (as defined by NIST SP 800-63):
- 1
- 2
- 3

{{</callout>}}


#### XPath Queries
{{< highlight xml "linenos=table" >}}
    Identity Assurance Level: 
        /*/system-characteristics/prop[@name="identity-assurance-level"]/@value
    Authenticator Assurance Level: 
        /*/system-characteristics/prop[@name="authenticator-assurance-level"]/@value
    Federation Assurance Level: 
        /*/system-characteristics/prop[@name="federation-assurance-level"]/@value
{{</ highlight >}}

---
### System Sensitivity Level 

A FedRAMP SSP in OSCAL defines the system's sensitivity level and supporting information with `security-sensitivity-level` and `security-impact-level` included together, rather than in a separate attachment like the SSP Appendix K FIPS Pub Level document, as illustrated below. The security sensitivity level should match the highest security impact level for the system’s confidentiality, integrity, and availability objectives for its `security-impact-level`.

{{< figure src="/img/ssp-figure-9.png" title="FedRAMP SSP template system sensitivity level." alt="Screenshot of the FIPS 199 system sensitivity level in the FedRAMP SSP template." >}}

#### OSCAL Representation
{{< highlight xml "linenos=table, hl_lines=15-16" >}}
<system-security-plan>
    <metadata>
        <!-- cut CSP Name -->
    </metadata>
    <system-characteristics>
        <!-- System Name & Abbreviation -->
        <system-name>System's Full Name</system-name>
        <system-name-short>System's Short Name or Acronym</system-name-short>        
        <!-- FedRAMP Unique Identifier -->
        <system-id identifier-type="http://fedramp.gov">F00000000</system-id>
        <!-- cut Service Model -->
        <!-- cut Deployment Model -->
        <!-- cut DIL Determination -->

        <!-- FIPS PUB 199 Level (SSP Attachment 10) -->
        <security-sensitivity-level>fips-199-moderate</security-sensitivity-level>

        <!--  cut --> 

        <security-impact-level>
            <security-objective-confidentiality>fips-199-moderate</security-objective-confidentiality>
            <security-objective-integrity>fips-199-moderate</security-objective-integrity>
            <security-objective-availability>fips-199-moderate</security-objective-availability>
        </security-impact-level>
         
        <!--  cut -->        
    </system-characteristics>
    <!--  cut -->     
</system-security-plan>
{{</ highlight >}}

<br />
{{<callout>}}

**OSCAL Allowed Values**

Valid values for security-sensitivity-level:
- fips-199-low
- fips-199-moderate
- fips-199-high

{{</callout>}}


#### XPath Queries
{{< highlight xml "linenos=table" >}}
    System Sensitivity Level:
        /*/system-characteristics/security-sensitivity-level
{{</ highlight >}}

**NOTES:**

-   The identified System Sensitivity Level governs which FedRAMP baseline applies. See the [*Importing the FedRAMP Baseline*](/documentation/ssp/3-working-with-oscal-files/#importing-the-fedramp-baseline) section for more information about importing the appropriate FedRAMP baseline.
-   The system sensitivity level should match the highest security impact level for the system’s confidentiality, integrity, and availability objectives, but in rare exceptions (e.g., when the AO specifies and overrides the expected security sensitivity level), they may differ. 

---
### System Information and Information Types

The `system-information` assembly and its defined `information-type` assemblies identify all of the information types that the system stores, processes, or transmits. FedRAMP requires digital authorization packages always use [the NIST SP 800-60 categorization system](https://doi.org/10.6028/NIST.SP.800-60v2r1) for information types. 
FedRAMP requires the  `categorization` for each `information-type` to identify the `information-type-id` with IDs. Because it is required by FedRAMP that [the NIST SP 800-60 categorization system](https://doi.org/10.6028/NIST.SP.800-60v2r1) is used for digital authorization packages, the `system` for each `information-type-id` must be `"https://doi.org/10.6028/NIST.SP.800-60v2r1"`.
Each information type has confidentiality, integrity, and availability (CIA) security impact levels recommended by NIST SP 800-60, which vary by information type. These recommended levels are set as the "base" values in the `base` field. However, an Authorizing Official may approve adjustments to these levels, documented by setting different values in the `selected` field. The `adjustment-justification` field must be used to provide a rationale whenever the `selected` FIPS-199 levels differ from the recommended `base` levels.

#### OSCAL Representation
{{< highlight xml "linenos=table" >}}
<system-security-plan>
    <metadata>
        <!-- cut CSP Name -->
    </metadata>
    <system-characteristics>
        <!-- System Name & Abbreviation -->
        <system-name>System's Full Name</system-name>
        <system-name-short>System's Short Name or Acronym</system-name-short>        
        <!-- FedRAMP Unique Identifier -->
        <system-id identifier-type="http://fedramp.gov">F00000000</system-id>
        <!-- cut Service Model -->
        <!-- cut Deployment Model -->
        <!-- cut DIL Determination -->

        <!-- FIPS PUB 199 Level (SSP Attachment 10) -->
        <security-sensitivity-level>fips-199-moderate</security-sensitivity-level>

        <!-- system-information -->
        <system-information>
            <information-type uuid="06ecba4f-db96-4491-a3a2-7febfa227435">
                <title>Information Type Name</title>
                <description>
                    <p>A description of the information.</p>
                </description>
                <categorization system="https://doi.org/10.6028/NIST.SP.800-60v2r1">
                    <information-type-id>C.2.4.1</information-type-id>
                </categorization>
                <confidentiality-impact>
                    <base>fips-199-moderate</base>
                    <selected>fips-199-moderate</selected>
                    <adjustment-justification>
                        <p>Required if the base and selected values do not match.</p>
                    </adjustment-justification>
                </confidentiality-impact>
                <integrity-impact>
                    <base>fips-199-moderate</base>
                    <selected>fips-199-moderate</selected>
                    <adjustment-justification>
                        <p>Required if the base and selected values do not match.</p>
                    </adjustment-justification>
                </integrity-impact>
                <availability-impact>
                    <base>fips-199-moderate</base>
                    <selected>fips-199-moderate</selected>
                    <adjustment-justification>
                        <p>Required if the base and selected values do not match.</p>
                    </adjustment-justification>
                </availability-impact>
            </information-type>
        </system-information>

        <!-- cut security-impact-level -->        
         
        <!--  cut -->        
    </system-characteristics>
    <!--  cut -->     
</system-security-plan>
{{</ highlight >}}

<br />
{{<callout>}}

**OSCAL Allowed Values**

Valid values for confidentiality-impact, integrity-impact, and availability-impact (base and selected fields):
- fips-199-low
- fips-199-moderate
- fips-199-high

Valid value for system attribute of the categorization field:
- https://doi.org/10.6028/NIST.SP.800-60v2r1

{{</callout>}}

#### XPath Queries
{{< highlight xml "linenos=table" >}}
    System Information:
        /*/system-characteristics/system-information
    System Information Types:
        /*/system-characteristics/system-information/information-type
    Information Categorization:
        /*/system-characteristics/system-information/information-type/categorization
    Information Categorization System (URI reference to standard used to categorize information types):
        /*/system-characteristics/system-information/information-type/categorization/@system 
    System Information Type Unique IDs:
        /*/system-characteristics/system-information/information-type/categorization/information-type-id
    Confidentiality Impact (base):
        /*/system-characteristics/system-information/information-type/confidentiality-impact/base
    Confidentiality Impact (selected):
        /*/system-characteristics/system-information/information-type/confidentiality-impact/selected
    Confidentiality Impact (adjustment justification):
        /*/system-characteristics/system-information/information-type/confidentiality-impact/adjustment-justification
    Integrity Impact (base):
        /*/system-characteristics/system-information/information-type/integrity-impact/base
    Integrity Impact (selected):
        /*/system-characteristics/system-information/information-type/integrity-impact/selected
    Integrity Impact (adjustment justification):
        /*/system-characteristics/system-information/information-type/integrity-impact/adjustment-justification
    Availability Impact (base):
        /*/system-characteristics/system-information/information-type/availability-impact/base
    Availability Impact (selected):
        /*/system-characteristics/system-information/information-type/availability-impact/selected
    Availability Impact (adjustment justification):
        /*/system-characteristics/system-information/information-type/availability-impact/adjustment-justification
{{</ highlight >}}

---
### System Status

The system status in the FedRAMP SSP template document is specified in the "Fully Operational as of" table cell illustrated in the figure below.  OSCAL has a `status` assembly that is used to describe the operational status of the system.  In addition, FedRAMP has defined the "fully-operational-date" extension `prop` that must be used to provide the date when the system became operational.  A system must be operational prior to seeking FedRAMP authorization, thus the "fully-operational-date" must be less than or equal to the current date.

{{< figure src="/img/ssp-figure-10.png" title="FedRAMP SSP template system status." alt="Screenshot of the system status information in the FedRAMP SSP template." >}}

#### OSCAL Representation
{{< highlight xml "linenos=table, hl_lines=18-24" >}}
<system-security-plan>
    <metadata>
        <!-- cut CSP Name -->
    </metadata>
    <system-characteristics>
        <!-- System Name & Abbreviation -->
        <system-name>System's Full Name</system-name>
        <system-name-short>System's Short Name or Acronym</system-name-short>        
        <!-- FedRAMP Unique Identifier -->
        <system-id identifier-type=“http://fedramp.gov/ns/oscal”>F00000000</system-id>
        <!-- cut Service Model -->
        <!-- cut Deployment Model -->
        <!-- cut DIL Determination -->

        <!-- FIPS PUB 199 Level (SSP Attachment 10) -->
        <security-sensitivity-level>fips-199-moderate</security-sensitivity-level>                   
        <!-- Fully Operational as of -->
        <status state="operational">
            <remarks>
                <p>If the status is “other”, the remarks field is required.</p>
                <p>Otherwise, it is optional.</p>
            </remarks>
        </status>
        <prop ns="https://fedramp.gov/ns/oscal" name="fully-operational-date" value="2024-11-05-00:00"/>        
        <!--  cut -->        
    </system-characteristics>
    <!--  cut -->     
</system-security-plan>
{{</ highlight >}}

<br />
{{<callout>}}

**OSCAL Allowed Values**

FedRAMP only accepts those in bold:
- **operational**
- under-development
- **under-major-modification**
- disposition
- **other**

{{</callout>}}

#### XPath Queries
{{< highlight xml "linenos=table" >}}
    System's Operational Status:
        /*/system-characteristics/status/@state
    Remarks on System's Operational Status:
        /*/system-characteristics/status/remarks/node()
    Fully Operational As Of Date:
        /*/system-characteristics/prop[@name="fully-operational-date"][@ns="https://fedramp.gov/ns/oscal"]/@value
{{</ highlight >}}

**NOTE:**

-   The value of the "fully-operational-date" `prop` must be a [date-with-timezone](https://pages.nist.gov/metaschema/specification/datatypes/#date-with-timezone) type.
-   If the status is "other", the remarks field is required. Otherwise, it is optional.
-   While under-development and disposition are valid OSCAL values, systems with either of these operational status values are not eligible for a FedRAMP Authorization.

---
### System Functionality

The system functionality in the FedRAMP SSP template document is specified in the “General System Description” table cell illustrated in the figure below. OSCAL has a `description` field within the `system-characteristics` assembly that is used to describe the system and its functionality.

{{< figure src="/img/ssp-figure-11.png" title="FedRAMP SSP template general system description." alt="Screenshot of the general system description information in the FedRAMP SSP template." >}}

#### OSCAL Representation
{{< highlight xml "linenos=table, hl_lines=19-24" >}}
<system-security-plan>
    <metadata>
        <!-- cut CSP Name -->
    </metadata>
    <system-characteristics>
        <!-- System Name & Abbreviation -->
        <system-name>System's Full Name</system-name>
        <system-name-short>System's Short Name or Acronym</system-name-short>        
        <!-- FedRAMP Unique Identifier -->
        <system-id identifier-type=“http://fedramp.gov/ns/oscal”>F00000000</system-id>
        <!-- cut Service Model -->
        <!-- cut Deployment Model -->
        <!-- cut DIL Determination -->

        <!-- FIPS PUB 199 Level (SSP Attachment 10) -->
        <security-sensitivity-level>fips-199-moderate</security-sensitivity-level>                   
        <!-- cut Fully Operational as of -->

        <!-- system functionality -->
        <description>
            <p>Describe the purpose and functions of this system here.</p>
            <!-- list of services/features in scope -->
            <!-– (use paragraph, list item, or table) -->          
        </description>

    </system-characteristics>
    <!--  cut -->     
</system-security-plan>
{{</ highlight >}}

#### XPath Queries
{{< highlight xml "linenos=table" >}}
    System Function or Purpose: First paragraph in description
        /*/system-characteristics/description/node()

{{</ highlight >}}

---
## Information System Owner

A `role` with an ID value of "system-owner" is required. Use the `responsible-party` assembly to associate this `role` with the `party` assembly containing the System Owner's information.  The `responsible-party` for a "system-owner" must be a `party` of type "person".

{{< figure src="/img/ssp-figure-12.png" title="FedRAMP SSP template information system owner." alt="Screenshot of the system owner  information in the FedRAMP SSP template." >}}

#### OSCAL Representation
{{< highlight xml "linenos=table" >}}
<metadata>
    <!-- cut -->
    <role id="system-owner"><!-- cut --></role>
    <location uuid="uuid-of-hq-location">
        <title>CSP HQ</title>
        <address type="work">
            <addr-line>1234 Some Street</addr-line>
            <city>Haven</city>
            <state>ME</state>
            <postal-code>00000</postal-code>
        </address>
    </location>
    <party uuid="uuid-of-csp" type="organization">
        <name>Cloud Service Provider (CSP) Name</name>
    </party>
    <party uuid="uuid-of-person-1" type="person">
        <name>[SAMPLE]Person Name 1</name>
        <prop name="job-title" value="Individual's Title"/> 
            <prop name="mail-stop" value="A-1"/>
                <email-address>name@example.com</email-address>
                <telephone-number>202-000-0000</telephone-number>
                <location-uuid>uuid-of-hq-location</location-uuid>
                <member-of-organization>uuid-of-csp</member-of-organization>
    </party>
    <responsible-party role-id="system-owner">
        <party-uuid>uuid-of-person-1</party-uuid>
    </responsible-party>
</metadata>
{{</ highlight >}}

<br />
{{<callout>}}

**OSCAL Allowed Values**

Required role ID:
- system-owner

{{</callout>}}

#### XPath Queries
{{< highlight xml "linenos=table" >}}
    System Owner's Name:
        /*/metadata/party[@uuid=[/*/metadata/responsible-party[@role-id="system-owner"]/party-uuid]]/name
    NOTE: Replace "name" with "email-address" or "telephone-number" above as needed.
    System Owner’s Address:
        /*/metadata/location[@uuid=/*/metadata/party[@uuid=[/*/metadata/responsible-party [@role-id="system-owner"]/party-uuid]]/location-uuid]/address/addr-line
    NOTE: Replace "addr-line" with "city", "state", or "postal-code" above as needed.
    System Owner's Title:
        /*/metadata/party[@uuid=[/*/metadata/responsible-party[@role-id="system-owner"]/party-uuid]]/prop[@name='job-title']/@value
    Company/Organization:
        /*/metadata/party[@uuid=/*/metadata/party[@uuid=[/*/metadata/responsible-party[@role-id="system-owner"]/party-uuid]]/member-of-organization]/name
{{</ highlight >}}

**NOTE:**

If no country is provided, FedRAMP tools will assume a US address.

---
## Federal Authorizing Officials

A `role` with an ID value of "authorizing-official" is required. Use the `responsible-party` assembly to associate this role with the `party` assembly containing the Authorizing Official's information.

##### Federal Agency Authorization Representation
{{< highlight xml "linenos=table" >}}
<metadata>
    <role id="authorizing-official">
        <title>Authorizing Official</title>
    </role>
    <party uuid="uuid-of-agency" type="organization">
        <name>Agency Name</name>
        <address type="work">
            <addr-line>Address Line</addr-line>
            <city>City</city>
            <state>ST</state>
            <postal-code>00000</postal-code>
            <country>US</country>
         </address>
    </party>
    <responsible-party role-id="authorizing-official">
        <party-uuid>uuid-of-agency</party-uuid>
    </responsible-party>
</metadata>
<!-- import -->
<system-characteristics>
    <!-- description -->
    <prop name="authorization-type" 
          ns="https://fedramp.gov/ns/oscal" 
          value="fedramp-agency" />
    <!-- prop -->
</system-characteristics>
{{</ highlight >}}

#### XPath Queries
{{< highlight xml "linenos=table" >}}
    FedRAMP Authorization Type:
        /*/system-characteristics/prop[@name="authorization-type"][@ns="https://fedramp.gov/ns/oscal"]/@value
    Authorizing Official:
        /*/metadata/party[@uuid=[/*/metadata/responsible-party[@role-id="authorizing-official"]/party-uuid]]/name
{{</ highlight >}}

---
## Assignment of Security Responsibilities

A `role` with an ID value of "information-system-security-officer" is required. Use the `responsible-party` assembly to associate this `role` with the `party` assembly containing the Information System Security Officer's information. The `responsible-party` for a "information-system-security-officer" must be a `party` of type "person".

{{< figure src="/img/ssp-figure-14.png" title="FedRAMP SSP template security point of contact." alt="Screenshot of the security point of contact information (e.g., ISSO) in the FedRAMP SSP template." >}}

<br />
{{<callout>}}
**NOTES ON ADDRESSES**

**Preferred Approach:** When multiple parties share the same address, such as multiple staff members at a company HQ, define the location once as a location assembly, then use the location-uuid field within each party assembly to identify the location of that individual or team.

**Alternate Approach:** If the address is unique to this individual, it may be included in the party assembly itself. 

**Hybrid Approach:** It is possible to include both a location-uuid and an address assembly within a party assembly. This may be used where multiple staff are in the same building but have different office numbers or mail stops. Use the location-uuid to identify the shared building, and only include a single addr-line field within the party's address assembly.

A tool developer may elect to always create a location assembly, even when only used once; however, tools must recognize and handle all of the approaches above when processing OSCAL files.

{{</callout>}}

#### OSCAL Representation
{{< highlight xml "linenos=table" >}}
<metadata>
    <!-- cut -->
    <role id="information-system-security-officer"><!-- cut -->
        <title>Information System Security Officer (or Equivalent)</title>
    </role>
    <location uuid="uuid-of-hq-location">
        <title>CSP HQ</title>
        <address type="work">
            <addr-line>1234 Some Street</addr-line>
            <city>Haven</city>
            <state>ME</state>
            <postal-code>00000</postal-code>
        </address>
    </location>
    <party uuid="uuid-of-csp" type="organization">
        <name>Cloud Service Provider (CSP) Name</name>
    </party>
    <party uuid="uuid-of-person-10" type="person">
        <name>[SAMPLE]Person Name 10</name>
        <prop name="job-title" value="Individual's Title"/>
        <email-address>name@org.domain</email-address>
        <telephone-number>202-000-0000</telephone-number>
        <location-uuid>uuid-of-hq-location</location-uuid>
        <member-of-organization>uuid-of-csp</member-of-organization>
    </party>
    <responsible-party role-id="information-system-security-officer">
        <party-uuid>uuid-of-person-10</party-uuid>
    </responsible-party>
</metadata>
{{</ highlight >}}

<br />
{{<callout>}}

**OSCAL Allowed Value**

Required Role ID:
- information-system-security-officer

{{</callout>}}

#### XPath Queries
{{< highlight xml "linenos=table" >}}
    ISSO POC Name:
        /*/metadata/party[@uuid=[/*/metadata/responsible-party[@role-id="information-system-security-officer"]/party-uuid]]/name
    NOTE: Replace "name" with "email-address" or "telephone-number" above as needed.
    ISSO POC’s Address:
        /*/metadata/location[@uuid=/*/metadata/party[@uuid=[/*/metadata/responsible-party [@role-id="information-system-security-officer"]/party-uuid]]/location-uuid]/address/addr-line
    NOTE: Replace "addr-line" with "city", "state", or "postal-code" above as needed.
    ISSO POC's Title:
        /*/metadata/party[@uuid=[/*/metadata/responsible-party[@role-id="information-system-security-officer"]/party-uuid]]/prop[@name='job-title']
    Company/Organization:
        /*/metadata/party[@uuid=/*/metadata/party[@uuid=[/*/metadata/responsible-party[@role-id="information-system-security-officer"]/party-uuid]]/member-of-organization]/name
{{</ highlight >}}

---

## Summary of SSP Roles Requirements

A FedRAMP OSCAL SSP must have "system-owner" `role` defined, an "authorizing-official" `role`, and an "information-system-security-officer" `role` defined.  The "system-owner" and "information-system-security-officer" roles must use the `responsible-party` assembly to associate the role to a `party` of type "person". For details, see the [System Owner](#information-system-owner) and [Assignment of Security Responsibilities](#assignment-of-security-responsibilities) sections.

The roles listed below are no longer required by FedRAMP:
- "authorizing-official-poc"
- "system-poc"
- "system-poc-management"
- "system-poc-technical"
- "system-poc-other"

If SSP authors include these optional roles in the SSP, they should give consideration to which `responsible-party` and corresponding `party` to associate with the role.  Generally, "poc" roles should be associated with a `party` of type "person".

---

## Data Centers

Each system must define at least two data centers. There must be exactly one primary data center, and there must be at least one alternate data center. Additionally, the country specified in a data center's address must be the United States. It must be in [ISO 3166 Alpha-2 format](https://pages.nist.gov/OSCAL-Reference/models/v1.1.2/system-security-plan/xml-reference/#/system-security-plan/metadata/location/address/country) two-letter country code format (i.e., "US" in all upper case).

#### OSCAL Representation
{{< highlight xml "linenos=table" >}}
<metadata>
    <!-- role -->
    <location uuid="uuid-of-primary-data-center">
       <title>Primary Data Center</title>
       <address>
          <addr-line>1234 Some Street</addr-line>
          <city>Haven</city>
          <state>ME</state>
          <postal-code>00000</postal-code>
          <country>US</country>
       </address>
       <prop name="type" class="primary" value="data-center"/>
    </location>
    <location uuid="uuid-of-alternate-data-center">
       <title>Secondary Data Center</title>
       <address>
          <addr-line>5678 Some Street</addr-line>
          <city>Haven</city>
          <state>ME</state>
          <postal-code>00000</postal-code>
          <country>US</country>
       </address>
       <prop name="type" class="alternate" value="data-center"/>
    </location>
    <!-- party -->
</metadata>
{{</ highlight >}}

#### XPath Queries
{{< highlight xml "linenos=table" >}}
    Number of Data Centers:
        count(/*/metadata/location[prop[@value eq 'data-center']]) > 1
    Number of Primary Data Centers:
        count(/*/metadata/location/prop[@value eq 'data-center'][@class eq 'primary']) = 1
    Number of Alternate Data Centers:
        count(/*/metadata/location/prop[@value eq 'data-center'][@class eq 'alternate']) > 0
    Data Center Country:
        /*/metadata/location[prop[@value eq 'data-center']]/address/country eq 'US'
{{</ highlight >}}

---
## Leveraged FedRAMP-authorized Services

If this system is leveraging the authorization of one or more systems, such as a SaaS running on an IaaS, each leveraged system must be represented within the system-implementation assembly. There must be one leveraged-authorization assembly and one matching component assembly for each leveraged authorization.

The leveraged-authorization assembly includes the leveraged system's name, point of contact (POC), and authorization date. The component assembly must be linked to the leveraged-authorization assembly using a property (prop) field with the name leveraged-authorization-uuid and the
UUID value of its associated leveraged-authorization assembly. The component assembly enables controls to reference it with the by-component responses described in the [*Control Implementation Descriptions*](/documentation/ssp/6-security-controls/#control-implementation-descriptions) section. The implementation-point property value must be set to "external".

If the leveraged system owner provides a UUID for their system, such as in an OSCAL-based Inheritance and Responsibility document (similar to a CRM), it should be provided as the inherited-uuid property value.

{{< figure src="/img/ssp-figure-15.png" title="FedRAMP SSP template leveraged FedRAMP-authorized services." alt="Screenshot of the leveraged FedRAMP-authorized service information in the FedRAMP SSP template." >}}

<br />
{{<callout>}}

**IMPORTANT FOR LEVERAGED SYSTEMS:**

While a leveraged system has no need to represent content here, its SSP must include special inheritance and responsibility information in the individual controls. See the [*Response: Identifying Inheritable Controls and Customer Responsibilities*](/documentation/ssp/6-security-controls/#response-identifying-inheritable-controls-and-customer-responsibilities) section for more information.

{{</callout>}}

#### OSCAL Representation
{{< highlight xml "linenos=table" >}}
<metadata>
    <!-- CSP name -->
    <party uuid="uuid-value">
        <name>Example IaaS Provider</name>
        <short-name>E.I.P.</short-name>
    </party>
</metadata>
<!-- cut import-profile, system-characteristics -->
<system-implementation>
    <leveraged-authorization uuid="uuid-value" >
        <title>Name of Underlying System</title>
        <!-- FedRAMP Package ID -->
        <prop name="leveraged-system-identifier" 
            ns="https://fedramp.gov/ns/oscal" 
            value="Package_ID value" />
        <prop ns="https://fedramp.gov/ns/oscal" name="authorization-type" 
              value="fedramp-agency"/>
        <prop ns="https://fedramp.gov/ns/oscal" name="impact-level" value="moderate"/>
        <link href="//path/to/leveraged_system_legacy_crm.xslt" />
        <link href="//path/to/leveraged_system_responsibility_and_inheritance.xml" />
        <party-uuid>uuid-of-leveraged-system-poc</party-uuid>
        <date-authorized>2015-01-01</date-authorized>
    </leveraged-authorization>
    <!-- CSO name & service description -->
    <component uuid="uuid-of-leveraged-system" type="leveraged-system">
        <title>Name of Leveraged System</title>
        <description>
            <p>Briefly describe leveraged system.</p>
        </description>
        <prop name="leveraged-authorization-uuid" 
              value="5a9c98ab-8e5e-433d-a7bd-515c07cd1497" />
        <prop name="inherited-uuid" value="11111111-0000-4000-9001-000000000001" />
        <prop name="implementation-point" value="external"/>
        <!-- FedRAMP prop extensions for table 6.1 columns -->
        <status state="operational"/>
    </component>
</system-implementation>
{{</ highlight >}}

<br />
{{<callout>}}

The title field must match an existing [FedRAMP authorized Cloud_Service_Provider_Package](https://raw.githubusercontent.com/18F/fedramp-data/master/data/data.json) property value.

A leveraged-system-identifier property must be provided within each leveraged-authorization field.  The value of this property must be from the same Cloud Service Provider as identified in the title field.


{{</callout>}}

#### XPath Queries
{{< highlight xml "linenos=table" >}}
    Name of first leveraged system:
        /*/system-implementation/leveraged-authorization[1]/title
    Name of first leveraged system CSO service (component):
        (//*/component/prop[@name="leveraged-authorization-uuid" and @value="uuid-of-leveraged-system"]/parent::component/title)[1]
    Description of first leveraged system CSO service (component):
        (//*/component/prop[@name="leveraged-authorization-uuid" and @value="uuid-of-leveraged-system"]/parent::component/description)[1]
    Authorization type of first leveraged system:
        /system-security-plan/system-implementation[1]/leveraged-authorization[1]/prop[@ns="https://fedramp.gov/ns/oscal" and @name="authorization-type"]/@value
    FedRAMP package ID# of the first leveraged system:
        /system-security-plan/system-implementation[1]/leveraged-authorization[1]/prop[@ns="https://fedramp.gov/ns/oscal" and @name="leveraged-system-identifier"]/@value
    Nature of Agreement for first leveraged system:
        (//*/component/prop[@name="leveraged-authorization-uuid" and @value="uuid-of-leveraged-system"]/parent::component/prop[@ns="https://fedramp.gov/ns/oscal" and @name="nature-of-agreement"]/@value)[1]
    FedRAMP impact level of the first leveraged system:
        /system-security-plan/system-implementation[1]/leveraged-authorization[1]/prop[@ns="https://fedramp.gov/ns/oscal" and @name="impact-level"]/@value
    Data Types transmitted to, stored or processed by the first leveraged system CSO:
        (//*/component/prop[@name="leveraged-authorization-uuid" and @value="uuid-of-leveraged-system"]/parent::component/prop[@ns="https://fedramp.gov/ns/oscal" and @name="interconnection-data-type"]/@value)
    Authorized Users of the first leveraged system CSO:
        //system-security-plan/system-implementation/user[@uuid="uuid-of-user"]
    Corresponding Access Level:
        //system-security-plan/system-implementation/user[@uuid="uuid-of-user"]/prop[@name="privilege-level"]/@value
    Corresponding Authentication method:
        //system-security-plan/system-implementation/user[@uuid="uuid-of-user"]/prop[@ns="https://fedramp.gov/ns/oscal" and @name="authentication-method"]/@value
{{</ highlight >}}

<br />
{{<callout>}}
Replace XPath predicate "[1]" with "[2]", "[3]", etc.
{{</callout>}}

---

## Users

A FedRAMP SSP must identify the users of the system by type, privilege, sensitivity level, the ID of the associated role, and a list of one or more authorized privileges.  The SSP must also provide the authentication method(s) used for each identified user.

### OSCAL Representation

{{< highlight xml "linenos=table" >}}
<system-implementation>
    <user uuid="system-admin-user-uuid">
        <title>System Administrator</title>
        <prop name="sensitivity" ns="https://fedramp.gov/ns/oscal" value="limited" />
        <prop name="type" value="external"/>
        <prop name="privilege-level" value="no-logical-access" />
        <role-id>system-admin-user</role-id>
        <authorized-privilege>
            <title>Full administrative access (root)</title>
            <function-performed>install and configure software</function-performed>
            <function-performed>OS updates, patches and hotfixes</function-performed>
            <function-performed>perform backups</function-performed>
        </authorized-privilege>
    </user>
</system-implementation>
{{</ highlight >}}

<br />

{{<callout>}}

**FedRAMP Extension:**

**OSCAL prop**
- name="type"

**OSCAL Allowed Values**

- internal
- external
- general-public

---

**OSCAL prop**
- name="privilege-level"

**OSCAL Allowed Values**

- privileged
- non-privileged
- no-logical-access

---

**FedRAMP Extension:**

prop (ns=“https://fedramp.gov/ns/oscal")
- name="sensitivity"

**FedRAMP Allowed Values**

- high-risk
- severe
- moderate
- limited
- not-applicable

---

**FedRAMP Extension:**

prop (ns=“https://fedramp.gov/ns/oscal")
- name="authentication-method"

**FedRAMP Allowed Values**

Values for `authentication-method` are not constrained.  However, SSP authors should provide values that are consistent with the authentication types identified in [NIST SP 800-63B](https://pages.nist.gov/800-63-3/sp800-63b.html#63bSec4-Table1).


{{</callout>}}

### XPath Queries

{{< highlight xml "linenos=table" >}}
Number of entries in the role table: count(/*/system-implementation/user)
Role: /*/system-implementation/user[1]/title
Replace "[1]" with "[2]", "[3]", etc.
Internal or External: /*/system-implementation/user[1]/prop[@name="type"]/@value
Privileged, Non-Privileged, or No Logical Access: /*/system-implementation/user[1]/prop[@name="privilege-level"]/@value
Sensitivity Level: /*/system-implementation/user[1]/prop[@name="sensitivity"][@ns= "https://fedramp.gov/ns/oscal"]/@value
Authentication method: /*/system-implementation/user[1]/prop[@name="authentication-method"][@ns="https://fedramp.gov/ns/oscal"]/@value
Authorized Privileges: /*/system-implementation/user[1]/authorized-privilege/title
count(/*/system-implementation/user[1]/authorized-privilege)
Functions Performed: /*/system-implementation/user[1]/authorized-privilege[1]/function-performed[1]
count(/*/system-implementation/user[1]/authorized-privilege[1]/function-performed)
{{</ highlight >}}

## External Systems and Services Not Having FedRAMP Authorization

FedRAMP authorized services should be used, whenever possible, since their risk is defined.  However, there are instances where CSOs have external systems or services that are not FedRAMP authorized.  In OSCAL, these external systems and services must be identified using `component` assemblies with additional FedRAMP namespace and class properties as shown in the OSCAL representation below.  

{{< figure src="/img/ssp-figure-17.png" title="FedRAMP SSP template external systems (not FedRAMP authorized)." alt="Screenshot of the external system information for non-FedRAMP authorized services in the FedRAMP SSP template." >}}

#### OSCAL Representation
{{< highlight xml "linenos=table" >}}
<!-- list any external connections as components in the system-characteristics -->
<component uuid="uuid-value" type="interconnection">
    <title>[EXAMPLE]External System / Service Name</title>
    <description>
        <p>Briefly describe the interconnection details.</p>
    </description>
    <!-- Props for table 7.1 columns -->
    <prop ns="https://fedramp.gov/ns/oscal" name="service-processor" 
            value="[SAMPLE] Telco Name"/>
        <prop ns="https://fedramp.gov/ns/oscal" name="interconnection-type" value="1" />
        <prop name="direction" value="incoming"/>
        <prop name="direction" value="outgoing"/>
        <prop ns="https://fedramp.gov/ns/oscal" name="nature-of-agreement" 
            value="contract" />
        <prop ns="https://fedramp.gov/ns/oscal" name="still-supported" value="yes" />
        <prop ns="https://fedramp.gov/ns/oscal" class="fedramp" 
            name="interconnection-data-type" value="C.3.5.1" />
        <prop ns="https://fedramp.gov/ns/oscal" class="fedramp" 
            name="interconnection-data-type" value="C.3.5.8" /> 
        <prop ns="https://fedramp.gov/ns/oscal" class="C.3.5.1" 
            name="interconnection-data-categorization" value="low" />
        <prop ns="https://fedramp.gov/ns/oscal" class="C.3.5.8" 
            name="interconnection-data-categorization" value="moderate" /> 
        <prop ns="https://fedramp.gov/ns/oscal" name="authorized-users" 
            value="SecOps engineers" />
        <prop ns="https://fedramp.gov/ns/oscal" class="fedramp" 
            name="interconnection-compliance" value="PCI SOC 2" />
        <prop ns="https://fedramp.gov/ns/oscal" class="fedramp" 
            name="interconnection-compliance" value="ISO/IEC 27001" />
        <prop ns="https://fedramp.gov/ns/oscal" name="interconnection-hosting-environment" 
            value="PaaS" />
        <prop ns="https://fedramp.gov/ns/oscal" name="interconnection-risk" value="None" />
        <prop name="isa-title" value="system interconnection agreement"/>
        <prop name="isa-date" value="2023-01-01T00:00:00Z"/>
        <prop name="ipv4-address" class="local" value="10.1.1.1"/>
        <prop name="ipv4-address" class="remote" value="10.2.2.2"/>
        <prop name="ipv6-address" value="::ffff:10.2.2.2"/>
        <prop ns="https://fedramp.gov/ns/oscal" name="information" 
            value="Describe the information being transmitted."/>
        <prop ns="https://fedramp.gov/ns/oscal" name="port" class="remote" value="80"/>
        <prop ns="https://fedramp.gov/ns/oscal" name="interconnection-security" 
            value="ipsec">
                <!-- cut ports, protocols -->
    <link href="#uuid-of-ICA-resource-in-back-matter" rel="isa-agreement" />                                    
    <!-- cut repeat responsible-party assembly for each required ICA role id -->
</component>
<!-- cut …. -->
<back-matter>
    <resource uuid="uuid-value">
        <title>[SAMPLE]Interconnection Security Agreement Title</title>
        <prop name="version" value="Document Version"/>
        <rlink href="./documents/ISAs/ISA-1.docx"/>
        <citation><!-- cut --></citation>
    </resource>
    <!-- repeat citation assembly for each ICA -->
</back-matter>
{{</ highlight >}}

### External System and Services (Queries)

Refer to the XPath queries below and corresponding notes for guidance on what targets in an OSCAL SSP should be used to represent each column of the "External Systems and Services Not Having FedRAMP Authorization" table in the legacy SSP template.

#### XPath Queries
{{< highlight xml "linenos=table" >}}
    Interconnection # for first external system:
        /*/system-implementation/component[@type='interconnection'][1]/ prop[@ns="https://fedramp.gov/ns/oscal" and @name="interconnection-type"]/@value
    System/Service/API/CLI Name:
        /*/system-implementation/component[@type='interconnection']/title
    Connection Details:
        /*/system-implementation/component[@type='interconnection'][1]/prop[@name="direction"]/@value
    Nature of Agreement for first external system:
        /*/system-implementation/component[@type='interconnection'][1]/ prop[@ns="https://fedramp.gov/ns/oscal" and @name="nature-of-agreement"]/@value
    Still Supported (Y/N):
        /*/system-implementation/component[@type='interconnection'][1]/ prop[@ns="https://fedramp.gov/ns/oscal" and @name="still-supported"]/@value
    Data Types:
        /*/system-implementation/component[@type='interconnection'][1]/prop[@ns="https://fedramp.gov/ns/oscal" and @name="interconnection-data-type"]/@value
    Data Categorization:
        /*/system-implementation/component[@type='interconnection'][1]/prop[@ns="https://fedramp.gov/ns/oscal" and @name="interconnection-data-categorization"]/@value
    Authorized Users:
        //system-security-plan/system-implementation/user[@uuid="uuid-of-user"]
    Corresponding Access Level:
        //system-security-plan/system-implementation/user[@uuid="uuid-of-user"]/prop @name="privilege-level"]/@value
    Other Compliance Programs:
        /*/system-implementation/component[@type='interconnection'][1]/prop[@ns="https://fedramp.gov/ns/oscal" and @name="interconnection-compliance"]/@value
    Description:
        /*/system-implementation/component[@type='interconnection'][1]/description
    Hosting Environment: 
        /*/system-implementation/component[@type='interconnection'][1]/prop[@ns="https://fedramp.gov/ns/oscal" and @name="interconnection-hosting-environment"]/@value
    Risk/Impact/Mitigation: 
        /*/system-implementation/component[@type='interconnection'][1]/prop[@ns="https://fedramp.gov/ns/oscal" and @name="interconnection-risk"]/@value
{{</ highlight >}}

<br />
{{<callout>}}
Replace XPath predicate "[1]" with "[2]", "[3]", etc.
{{</callout>}}

---
## Illustrated Architecture and Narratives

### Authorization Boundary

The OSCAL approach to this type of diagram is to treat the image data as either a linked or base64-encoded `resource` in the `back-matter` section of the OSCAL file, then reference the diagram using the `link` field. The narrative describing the system architecture must be provided in the `description` field of the `authorization-boundary` assembly.

{{< figure src="/img/ssp-figure-19.png" title="FedRAMP SSP template authorization boundary sections." alt="Screenshot of the authorization boundary information in the FedRAMP SSP template." >}}

#### OSCAL Representation
{{< highlight xml "linenos=table" >}}
<system-characteristics>
    <!-- leveraged-authorization -->
    <authorization-boundary>
        <!-- 8.2 Narrative (Boundary) -->
        <description>
            <p>A holistic, top-level explanation of the FedRAMP authorization boundary.</p>
        </description>
        <!-- 8.1 Illustrated Architecture (Boundary) -->
        <diagram uuid="uuid-value">
            <description><p>A diagram-specific explanation.</p></description>
            <!-- href can reference a resource in the back-matter OR link the diagram inline  -->
            <link href="#d2eb3c18-6754-4e3a-a933-03d289e3fad1" rel="diagram" />
            <!-- OR -->
            <link href="./diagram.png" rel="diagram" />
            <caption>Authorization Boundary Diagram</caption>
        </diagram>
        <!-- repeat diagram assembly for each additional boundary diagram -->
    </authorization-boundary>
    <!-- network-architecture -->
</system-characteristics>

<!-- cut -->

<back-matter>
    <resource uuid="uuid-of-boundary-diagram-1">
        <description><p>The primary authorization boundary diagram.</p></description>
        <base64 filename="architecture-main.png" media-type="image/png">00000000</base64>
    </resource>
</back-matter>
{{</ highlight >}}


#### XPath Queries
{{< highlight xml "linenos=table" >}}
    Overall Description:
        /*/system-characteristics/authorization-boundary/description/node()
    Count the Number of Diagrams (There should be at least 1):
        count(/*/system-characteristics/authorization-boundary/diagram)
    Link to First Diagram:
        /*/system-characteristics/authorization-boundary/diagram[1]/link/@href


    If the diagram link points to a resource within the OSCAL file:
        /*/back-matter/resource[@uuid="uuid-of-boundary-diagram"]/base64
    OR:
        /*/back-matter/resource[@uuid="uuid-of-boundary-diagram-1"]/rlink/@href
    Diagram-specific Description:
        /*/system-characteristics/authorization-boundary/diagram[1]/description/node()
{{</ highlight >}}

<br />
{{<callout>}}
Replace XPath predicate "[1]" with "[2]", "[3]", etc.
{{</callout>}}

---
### Network Architecture

Consistent with the [*Authorization Boundary*](#authorization-boundary) guidance, the OSCAL approach to network architecture diagrams is to treat image data as either a linked or base64-encoded `resource` in the `back-matter` section of the OSCAL file, then reference the diagram using the `link` field. The narrative describing the network architecture must be provided in the `description` field of the `network-architecture` assembly.

{{< figure src="/img/ssp-figure-19.png" title="FedRAMP SSP template network architecture." alt="Screenshot of the network architecture information in the FedRAMP SSP template." >}}

#### OSCAL Representation
{{< highlight xml "linenos=table" >}}
<system-characteristics>
    <!-- authorization-boundary -->
    <network-architecture>
        <!-- 8.2 Narrative (Network) -->
        <description>
            <p>A holistic, top-level explanation of the system's network.</p>
        </description>
        <!-- 8.1 Illustrated Architecture (Network) -->
        <diagram uuid="uuid-value">
            <description><p>A diagram-specific explanation.</p></description>
            <!-- href can reference a resource in the back-matter OR link the diagram inline  -->
            <link href="#d2eb3c18-6754-4e3a-a933-03d289e3fad2" rel="diagram" />
            <!-- OR -->
            <link href="./diagram.png" rel="diagram" />
            <caption>Network Diagram</caption>
        </diagram>
        <!-- repeat diagram assembly for each additional network diagram -->
    </network-architecture>
    <!-- data-flow -->
</system-characteristics>


<!-- cut -->


<back-matter>
    <!-- citation -->
    <resource uuid=" uuid-of-network-diagram-1">
        <description><p>The primary network architecture diagram.</p></description>
        <rlink href="./diagrams/network.png" media-type="image/png"/>
    </resource>
</back-matter>
{{</ highlight >}}


#### XPath Queries
{{< highlight xml "linenos=table" >}}
    Overall Description:
        /*/system-characteristics/network-architecture/description/node()
    Count the Number of Diagrams (There should be at least 1):
        count(/*/system-characteristics/network-architecture/diagram)
    Link to First Diagram:
        /*/system-characteristics/network-architecture/diagram[1]/link/@href


    If the diagram link points to a resource within the OSCAL file:
        /*/back-matter/resource[@uuid="uuid-of-network-diagram-1"]/base64
    OR:
        /*/back-matter/resource[@uuid="uuid-of-network-diagram-1"]/rlink/@href
    First Diagram Description:
        /*/system-characteristics/network-architecture/diagram[1]/description/node()
{{</ highlight >}}

<br />
{{<callout>}}
Replace XPath predicate "[1]" with "[2]", "[3]", etc.
{{</callout>}}

---
### Data Flow

Consistent with the [*Authorization Boundary*](#authorization-boundary) guidance, the OSCAL approach to data flow diagrams is to treat image data as either a linked or base64-encoded `resource` in the `back-matter` section of the OSCAL file, then reference the diagram using the `link` field. The narrative describing the data flows must be provided in the `description` field of the `data-flow` assembly.

{{< figure src="/img/ssp-figure-19.png" title="FedRAMP SSP template data flow." alt="Screenshot of data flow information in the FedRAMP SSP template." >}}

#### OSCAL Representation
{{< highlight xml "linenos=table" >}}
<system-characteristics>
    <!-- data-flow -->
    <data-flow>
        <!-- 8.2 Narrative (Data Flow) -->
        <description>
            <p>A holistic, top-level explanation of the system's data flows.</p>
        </description>
        <!-- 8.1 Illustrated Architecture (Data Flow) -->
        <diagram uuid="uuid-value">
            <description><p>A diagram-specific explanation.</p></description>
            <!-- href can reference a resource in the back-matter OR link the diagram inline  -->
            <link href="#d2eb3c18-6754-4e3a-a933-03d289e3fad3" rel="diagram" />
            <!-- OR -->
            <link href="./diagram.png" rel="diagram" />
            <caption>Data Flow Diagram</caption>
        </diagram>
        <!-- repeat diagram assembly for each additional data flow diagram -->
    </data-flow>
    <!-- network-architecture -->
</system-characteristics>   

<!-- cut -->

<back-matter>
    <!-- citation -->
    <resource uuid="uuid-of-dataflow-diagram-1">
        <description><p>The primary data flow diagram.</p></description>
        <base64 filename="data-flow-1.png" media-type="image/png">
            0000<!-- base64 cut -->0000
        </base64>
    </resource>
</back-matter>
{{</ highlight >}}


#### XPath Queries
{{< highlight xml "linenos=table" >}}
    Overall Description:
        /*/system-characteristics/data-flow/description/node()
    Count the Number of Diagrams (There should be at least 1):
        count(/*/system-characteristics/data-flow/diagram)
    Link to First Diagram:
        /*/system-characteristics/data-flow/diagram[1]/link/@href

    If the diagram link points to a resource within the OSCAL file:
        /*/back-matter/resource[@uuid="uuid-of-dataflow-diagram-1"]/base64
    OR:
        /*/back-matter/resource[@uuid="uuid-of-dataflow-diagram-1"]/rlink/@href
    First Diagram Description:
        /*/system-characteristics/data-flow/diagram[1]/description/node()
{{</ highlight >}}

<br />
{{<callout>}}
Replace XPath predicate "[1]" with "[2]", "[3]", etc.
{{</callout>}}

---
## Ports, Protocols and Services

Entries in the ports, protocols, and services table are represented as component assemblies, with the component-type flag set to "service". Use a protocol assembly for each protocol associated with the service. For a single port, set the port-range start flag and end flag to the same value.

{{< figure src="/img/ssp-figure-20.png" title="FedRAMP SSP template ports, protocols, and services." alt="Screenshot of the ports, protocols, and services information in the FedRAMP SSP template." >}}

#### OSCAL Representation
{{< highlight xml "linenos=table" >}}
<system-implementation>
    <!-- user -->
    <component uuid="uuid-of-service" type="service">
        <title>[SAMPLE]Service Name</title>
        <description><p>Describe the service</p></description>
        <purpose>Describe the purpose for which the service is needed.</purpose>
        <link href="uuid-of-component-used-by" rel="used-by" />
        <link href="uuid-of-component-provided-by" rel="provided-by" />
        <status state="operational" />
        <protocol name="http">
            <port-range start="80" end="80" transport="TCP"/>
        </protocol>
        <protocol name="https">
            <port-range start="443" end="443" transport="TCP"/>
        </protocol>
    </component>
    <!-- Repeat the component assembly for each row in Table 9.1 -->
    <!-- system-inventory -->
</system-implementation>
{{</ highlight >}}


#### XPath Queries
{{< highlight xml "linenos=table" >}}
    Service (1st service):
        /*/system-implementation/component[@type='service'][1]/title
    Ports: Start (1st service, 1st protocol, 1st port range):
        /*/system-implementation/component[@type='service'][1]/protocol[1]/port-range[1]/@start
    Ports: End (1st service, 1st protocol, 1st port range):
        /*/system-implementation/component[@type='service'][1]/protocol[1]/port-range[1]/@end
    Ports: Transport (1st service, 1st protocol, 1st port range):
        /*/system-implementation/component[@type='service'][1]/protocol[1]/port-range[1]/@transport
    Protocol (1st service, 1st protocol):
        /*/system-implementation/component[@type='service'][1]/protocol[1]/@name
    Purpose (1st service):
        /*/system-implementation/component[@type='service'][1]/purpose
    Used By (1st service):
        /*/system-implementation/component[@uuid='uuid-of-component-used-by']/title
{{</ highlight >}}

<br />
{{<callout>}}
Replace XPath predicate "[1]" with "[2]", "[3]", etc.
{{</callout>}}

---
## Cryptographic Modules Implemented for Data-in-Transit (DIT) 

OSCAL's component model treats independent validation of products and services as if that validation were a separate component. This means when using components with FIPS 140 validated cryptographic modules, there must be two component assemblies:

-   **The Validation Definition**: A component that provides details about the validation.

-   **The Product Definition**: A component that describes the hardware or software product.

The validation definition is a component that provides details about the independent validation. Its type must have a value of "validation". In the case of FIPS 140 validation, this must include a link field with a rel value set to "validation-details". This link must point to the cryptographic module's entry in the NIST Computer Security
Resource Center (CSRC) [Cryptographic Module Validation Program Database](https://csrc.nist.gov/projects/cryptographic-module-validation-program/validated-modules/search).

The product definition is a product with a cryptographic module. It must contain all of the typical component information suitable for reference by inventory-items and control statements. It must also include a link field with a rel value set to "validation" and an href value containing
a URI fragment. The fragment must start with a hashtag (#) and include the UUID value of the validation component. This links the two together.

{{< figure src="/img/ssp-figure-21.png" title="FedRAMP SSP template cryptographic modules table (data-in-transit)." alt="Screenshot of the cryptographic modules table (data-in-transit) in the FedRAMP SSP template." >}}

##### Component Representation: Example Product with FIPS 140-2 Validation
{{< highlight xml "linenos=table" >}}
<!-- system-characteristics -->
<system-implementation>
    <!-- user -->
    <!-- Minimum Required Components -->
    
    <!-- FIPS 140-2 Validation Certificate Information -->
    <!-- Include a separate component for each relevant certificate -->
    <component uuid="uuid-value" type="validation">
        <title>Module Name</title>
        <description><p>FIPS 140-2 Validated Module</p></description>
        <prop ns="https://fedramp.gov/ns/oscal" name="asset-type" 
              value="cryptographic-module" />
        <prop ns="https://fedramp.gov/ns/oscal" name="vendor-name" 
              value="CM Vendor"/>
        <prop ns="https://fedramp.gov/ns/oscal" name="cryptographic-module-usage" 
              value="data-in-transit"/>
        <prop name="validation-type" value="fips-140-2"/>
        <prop name="validation-reference" value="0000"/>
        <link href="https://csrc.nist.gov/projects/cryptographic-module-validation-program/Certificate/0000" rel="validation-details" />
        <status state="operational" />
    </component>
    
    <!-- FIPS 140-2 Validated Product -->
    <component uuid="uuid-value" type="software" >
        <title>Product Name</title>
        <description><p>A product with a cryptographic module.</p></description>
        <link href="#uuid-of-validation-component" rel="validation" />
        <status state="operational" />
    </component>
    
    <!-- service -->
</system-implementation>
<!-- control-implementation -->
{{</ highlight >}}

---
## Cryptographic Modules Implemented for Data-at-Rest (DAR)

The approach is the same as in the [*cryptographic module data-in-transit*](#cryptographic-modules-implemented-for-data-in-transit-dit) section.

{{< figure src="/img/ssp-figure-22.png" title="FedRAMP SSP template cryptographic modules table (data-at-rest)." alt="Screenshot of the cryptographic modules table (data-at-rest) in the FedRAMP SSP template." >}}

##### Component Representation: Example Product with FIPS 140-2 Validation
{{< highlight xml "linenos=table" >}}
<!-- system-characteristics -->
<system-implementation>
    <!-- user -->
    <!-- Minimum Required Components -->
    
    <!-- FIPS 140-2 Validation Certificate Information -->
    <!-- Include a separate component for each relevant certificate -->
    <component uuid="uuid-value" type="validation">
        <title>Module Name</title>
        <description><p>FIPS 140-2 Validated Module</p></description>
        <prop ns="https://fedramp.gov/ns/oscal" name="asset-type" 
              value="cryptographic-module" />
        <prop ns="https://fedramp.gov/ns/oscal" name="vendor-name" 
              value="CM Vendor"/>
        <prop ns="https://fedramp.gov/ns/oscal" name="cryptographic-module-usage" 
              value="data-at-rest"/>
        <prop name="validation-type" value="fips-140-2"/>
        <prop name="validation-reference" value="0000"/>
        <link href="https://csrc.nist.gov/projects/cryptographic-module-validation-program/Certificate/0000" rel="validation-details" />
        <status state="operational" />
    </component>
    
    <!-- FIPS 140-2 Validated Product -->
    <component uuid="uuid-value" type="software" >
        <title>Product Name</title>
        <description><p>A product with a cryptographic module.</p></description>
        <link href="#uuid-of-validation-component" rel="validation" />
        <status state="operational" />
    </component>
    
    <!-- service -->
</system-implementation>
<!-- control-implementation -->
{{</ highlight >}}
