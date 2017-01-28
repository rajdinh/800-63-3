<a name="sec5"></a>

<div class="breaker"></div>

## 5. Determining IAL, AAL, and FAL

*This section is informative.*

OMB [[M-04-04]](#M-04-04) requires agencies to select the transaction or system authentication LOA by conducting a risk assessment to determine the potential impact of an authentication error. In the context of M-04-04, LOA is defined as:

1. The degree of confidence in the vetting process used to establish the identity of the individual to whom the authenticator was issued; and
2. The degree of confidence that the individual who uses the credential is the individual to whom the credential was issued.

Per M-04-04, once an LOA is selected, the agency is required to follow the identity proofing, credentialing, and federation technical requirements specified by NIST at that level. For example, if the impact of an authentication error results in an application assessed at LOA3, the agency must identity proof and provide authenticators that meet the requirements for LOA3.

However, in today's digital services, combining proofing and authenticator requirements into a single bundle sometimes has unintended consequences and can put unnecessary implementation burden upon the implementing entity. It is quite possible that an agency can deliver the most effective set of identity services by assessing the risk and impacts of failures for each individual component of digital authentication, rather than as a single, all-encompassing LOA. An authentication error is not a singleton that drives all requirements. This guideline recommends that agencies meet M-04-04 requirements by separately evaluating the impact that could result from (1) a true authentication error (a false claimant using a credential that is not rightfully theirs) and (2) an identity proofing error. From the perspective of an identity proofing failure, there are two dimensions of potential risk:

*  The impact of providing a service to the wrong subject (e.g., an attacker successfully proofs as someone else).
*  The impact of excessive identity proofing (i.e., collecting and securely storing more information about a person than is required to successfully provide the digital service).

As such, agencies should assess the risk of authentication and proofing errors separately to determine the appropriate technical solutions that should be applied to their system. 

### 5.1. <a name="5-1"></a> Categories of Assurance Levels

To determine the process and system requirements that reduce the risk of an authentication or identity proofing failure, this guideline introduces distinct categories of assurance which drive the technical solutions an agency may implement. They are:

* IAL - The robustness of the identity proofing process to confidently determine the identity of an individual.
* AAL - The robustness of the authentication process itself, and the binding between an authenticator and the identifier of a specific individual.
* FAL - The robustness of the assertion protocol utilized by the federation to communicate authentication and attribute information (if applicable) to an RP. FAL is optional as not all digital systems will leverage federated identity architectures.

A summary of each of the identity, authenticator, and federation assurance levels is provided below.


| Identity Assurance Level |
|:----------------------|
| **IAL1** - At IAL1, attributes, if any, are self-asserted or should be treated as self-asserted.|
| **IAL2** - IAL2 introduces the need for either remote or in-person identity proofing. IAL2 requires identifying attributes to have been verified in person or remotely using, at a minimum, the procedures given in [[SP 800-63A]](sp800-63a.html).|
| **IAL3** - At IAL3, in-person identity proofing is required. Identifying attributes must be verified by an authorized representative of the CSP through examination of physical documentation as described in [[SP 800-63A]](sp800-63a.html).|

|Authenticator Assurance Level|
|:----------------------|
|**AAL1** - AAL1 provides some assurance that the claimant controls an authenticator registered to the subscriber. AAL1 requires single-factor authentication using a wide range of available authentication technologies. Successful authentication requires that the claimant prove possession and control of the authenticator(s) through a secure authentication protocol.|
| **AAL2** - AAL2 provides high confidence that the claimant controls authenticator(s) registered to the subscriber. Proof of possession and control of two different authentication factors is required through a secure authentication protocol. Approved cryptographic techniques are required at AAL2 and above.|
|**AAL3** - AAL3 provides very high confidence that the claimant controls authenticator(s) registered to the subscriber. Authentication at AAL3 is based on proof of possession of a key through a cryptographic protocol. AAL3 is like AAL2 but also requires a "hard" cryptographic authenticator that provides verifier impersonation resistance.|

|Federation Assurance Level|
|:----------------------|
|**FAL1** - FAL1 permits the RP to receive a bearer assertion from an identity provider (IdP). The assertion must be signed by the IdP using approved cryptography.|
|**FAL2** - FAL2 adds the requirement that the assertion be encrypted using approved cryptography such that the RP is the only party that can decrypt it.|
|**FAL3** - FAL3 requires the subscriber to present proof of possession of a cryptographic key referenced in the assertion in addition to the assertion artifact itself. The assertion must be signed using approved cryptography and encrypted to the RP using approved cryptography.|

When described generically or bundled, this guideline will refer to the combination of IAL, AAL, and FAL as **_xAL_**.

A risk assessment to determine each xAL is recommended. [Section 5.3](#CYOA) provides details on how agencies may going about making xAL selections. 


### 5.2. Mapping xAL to M-04-04 LOAs 

This guideline introduces a model where individual xALs can be selected without requiring parity to each other. While options exist to select varying xALs for a system, in many instances the same level will be chosen for all xALs. This provides an opportunity to directly map to the LOAs specified in M-04-04. [Table 5-1](#63sec5-Table1) shows strict adherence to M-04-04 by mapping the corresponding Identity, Authenticator, and Federation Assurance Levels to LOA.

<a name="63sec5-Table1"></a>

<div class="text-center" markdown="1">

**Table 5-1. Mapping xAL to Legacy M-04-04 Requirements**

</div>

| M-04-04 Level of Assurance (LOA) | Minimum Required Identity Assurance Level (IAL)| Minimum Required Authenticator Assurance Level (AAL) | Minimum Required Federation Assurance Level (FAL)
|:------------------:|:-----------------------------:|:------------------------:|:------------------------:|
| 1 | 1 | 1 | 1
| 2 | 2 | 2 | 2
| 3 | 2 | 2 | 2
| 4 | 3 | 3 | 3

> Note: LOA2 requirements are now equivalent to LOA3.

The ability to combine varying xALs offers significant flexibility to agencies, but not all combinations are possible due to the nature of the data collected from an individual and the authenticators to protect that data. [Table 5-2](#63sec5-Table2) details valid combinations of IAL and AAL to ensure personal information remains protected by MFA.

<a name="63sec5-Table2"></a>

<div class="text-center" markdown="1">

**Table 5-2. Acceptable combinations of IAL and AAL**

</div>


| | AAL1 | AAL2 | AAL3 |
|:-:|:-:|:-:|:-:|
| **IAL1: Without personal data** | Allowed | Allowed | Allowed |
| **IAL1: With personal data** | **NO** | Allowed | Allowed |
| **IAL2** |  **NO** | Allowed | Allowed |
| **IAL3** |  **NO** | Allowed | Allowed |

> Note: Per Executive Order 13681 [[EO 13681]](#EO13681), the release of personal data requires protection with MFA, even if the personal data is self-asserted and not validated. When the transaction does not make personal data accessible, authentication may occur at AAL1, although providing an option for the user to choose stronger authentication is recommended. In addition, it may be possible at IAL1 to self-assert information that is not personal, in which case AAL1 is acceptable.

 
### 5.3. <a name="CYOA"></a>Selecting the Appropriate xAL

Agency mission and risk tolerance will drive the most advantageous selection of xALs to minimize risk. This could include the selection of an IAL that is lower than the selected AAL. For example, suppose an agency establishes a "health tracker" application. In line with the terms of [EO 13681](#EO13681) requiring "...that all agencies making personal data accessible to citizens through digital applications require the use of multiple factors of authentication...", the agency is required to implement MFA at AAL2 or AAL3. 

The EO also requires agencies employ "...an effective identity proofing process, as appropriate" when personal information is released. This does not mean that proofing at IAL2 or IAL3 (to match the required AAL) is necessary. In the above example, there may be no need for the agency system to know who the user actually is. Therefore, an 'effective proofing process' would be to not proof at all. In the past, an LOA3 assessment would equate to identity proofing the user at IAL2. This is no longer necessary and the agency is discouraged from performing any identity proofing, allowing the user of the health tracker system to be pseudonymous.

Separating IAL and AAL also works for traditional M-04-04 LOA use cases, such as [[HSPD-12]](#HSPD-12) requirements for federal employees to obtain a Personal Identity Verification (PIV) smart card. The requirement for PIV is that agencies meet LOA4; therefore, an authenticator at AAL3 **and** identity proofing at IAL3 sufficiently meets the LOA4 requirements.

> Note: An agency can accept a higher assurance level than those required in the table above. For example, in a federated transaction, an agency can accept an IAL3 identity if their application is assessed at IAL2. The same holds true for authenticators; stronger authenticators can be used at RPs that have lower authenticator requirements. However, RPs will have to ensure that this only occurs in federated scenarios with appropriate privacy protections by the CSP such that only attributes that have been requested by the RP and authorized by the subscriber are provided to the RP and that excessive personal information does not leak from the credential or an assertion. See [privacy requirements](./sp800-63c.html#sec9) in SP 800-63C for more details.

<!---->

> Note: Since agencies are encouraged to consider each distinct element of assurance, the notion of the 'low watermark' to determine LOA no longer applies. An IAL1/AAL2 application should not be considered any less secure or privacy enhancing than an IAL2/AAL2 application. The only difference between these applications is the amount of proofing required, which may not impact the security and privacy of each application. That said, if an agency incorrectly determines the xAL, security and privacy could very well be impacted.

#### 5.3.1. Business Process vs. Online Transaction

An online transaction may not be equivalent to a complete business process that requires offline processing, or online processing in a completely segmented system. In selecting the appropriate assurance levels, the agency should assess the risk associated with online transactions they are offering via the digital service, not the entire business process associated with the provided benefit or service. For example, in an online survey, sensitive PII may be collected, but it is never made available online to the person after the information is submitted. In this instance, it is important for the information to be carefully protected in backend systems, but there is no reason to identity proof or even authenticate the user providing the information. The online transaction is solely a submission of the data. The entire business process may require a significant amount of data validation, without ever needing to know if the correct person submitted the information. In this scenario, there is no need for any identity proofing nor authentication.

Another example where the assessed risk could differ if the agency evaluated the entire business process rather than the online transaction requirements is a digital service that accepts resumes to apply for open job postings. In this use case, the digital service allows any individual to submit a resume on behalf of anyone else, and in subsequent visits to the site, access the resume for various purposes. Since the resume information is provided in later sessions, and is likely to contain PII, the agency must select an AAL that requires MFA. In this case, the requirements of [EO 13681](#eo13681) apply and the application must provide at least AAL2. However, the identity proofing requirements remain unclear. The entire business process of examining a resume and ultimately hiring and onboarding a person requires a significant amount of identity proofing. The agency needs a high level of confidence that the job applicant is in fact the subject of the resume submitted online if a decision to hire is made. Yet this level of proofing is not required to submit the resume online. Identity proofing is not required to complete the digital transaction successfully. Identity proofing the submitter would create more risk than required in the online system as excess personal information would be collected when no such information is needed for the portion of the hiring process served by the digital job application portal. Therefore, the most appropriate IAL selection would be 1. There is no need to identity proof the user to successfully complete the online transaction. While, there would be significant impact if the entire business process failed to correctly identity proof the person - a job may be offered to a fraudulent applicant - the requirement for identity proofing later in the process need not be applied to the online system.
 
#### <a name="AAL_CYOA"></a> 5.3.2. Selecting AAL

The AAL decision tree in [Figure 5-1](#63Sec5-Figure1) offers agencies a possible path to determine the most appropriate authentication requirements for their digital service offering. This analysis is not intended to substitute for M-04-04 risk assessments or any other risk management process in use by the agency. Nor is it expected to cover all the identity service needs of each agencies unique mission. Descriptions follow for specific steps that fall outside the risk management process documented in M-04-04.

The AAL selection does not mean the digital service provider will need to issue authenticators themselves. More information of whether the agency can federate or not is provided in [Section 5.4](#toFedorNotToFed). 

<a name="63Sec5-Figure1"></a>
<div class="text-center" markdown="1">
<img src="sp800-63-3/media/AAL_CYOA.png" alt="AAL Choose Your Own" style="width:1000px;height:1195px;;min-width: 1000px;min-height: 1195px;"/>

**Figure 5-1 - Selecting AAL**
</div>

<div class="text-left" markdown="1">
<table style="width:100%">
  <tr>
    <td><img src="sp800-63-3/media/aal-step1.png" alt="AAL Step 1"/></td>
  </tr>
  <tr>
   <td>Step 1 asks agencies to look at the potential impacts of an authentication failure. In other words, what would occur if an unauthorized user accessed one or more valid user accounts? Risk should be considered from the perspective of the organization and to a valid user, since one may not be negatively impacted while the other could be significantly harmed. The risk assessment process of M-04-04 and any agency specific risk management process should commence from this step.</td> 
  </tr>
  <tr>
    <td><img src="sp800-63-3/media/aal-step2.png" alt="AAL Step 2"/></td>
  </tr>
  <tr>

   <td>MFA is required when any personal information is made available online. Since the other paths in this decision tree already drive the agency to an AAL that requires MFA, the question regarding personal information is only raised at this point. That said, personal information release at all AALs should be considered when performing the risk assessment. An important point at this step is that the collection of personal information, if not made available online, does not need to be validated or verified to require an AAL of 2 or higher. Release of even self-asserted personal information requires account protection via MFA. Even though self-asserted information can be falsified, most users will provide accurate information to benefit from the digital service. As such, self-asserted data must be protected appropriately.</td> 

  </tr>
  
  </table>
</div>


#### <a name="IAL_CYOA"></a> 5.3.3. Selecting IAL

The IAL decision tree in [Figure 5-2](#63Sec5-Figure2) offers agencies a possible path to determine the most appropriate identity proofing requirements for their digital service offering. This analysis is not intended to be a substitute for M-04-04 risk assessments or any other risk management process in use by the agency. Nor is it expected to cover all the identity service needs of each agencies unique mission. Descriptions follow for specific steps that fall outside the required risk management process documented in M-04-04.

The IAL selection does not mean the digital service provider will need to perform the proofing themselves. More information on whether the agency can federate or not is provided in [Section 5.4](#toFedorNotToFed). 

<a name="63Sec5-Figure2"></a>
<div class="text-center" markdown="1">
<img src="sp800-63-3/media/IAL_CYOA.png" alt="IAL Choose Your Own" style="width:1000px;height:1195px;;min-width: 1000px;min-height: 1195px;"/>

**Figure 5-2 - Selecting IAL**
</div>


<div class="text-left" markdown="1">
<table style="width:100%">
  <tr>
    <td><img src="sp800-63-3/media/ial-step1.png" alt="IAL Step 1"/></td>
  </tr>
  <tr>
   <td>The risk assessment and selection of IAL can be short circuited by answering this question first. If the service does not require any personal information to execute any digital transactions, the IAL of the system can be set to 1.</td> 
  </tr>
  <tr>
    <td><img src="sp800-63-3/media/ial-step2.png" alt="IAL Step 2"/></td>
  </tr>
  <tr>
   <td>If personal information is needed, the RP needs to determine if validated and verified attributes are required, or if self-asserted attributes are acceptable. If even a single validated and verified attribute is needed, then the provider will need to accept attributes that have been IAL2 or IAL3 proofed. Again, the selection of IAL can be short circuited to IAL1 if the agency can deliver the digital service with self-asserted attributes only.</td> 
  </tr>
  <tr>
    <td><img src="sp800-63-3/media/ial-step3.png" alt="IAL Step 3"/></td>
  </tr>
  <tr>
   <td>At this point, the agency understands that some level of proofing is required. Step 3 is intended to look at the potential impacts of an identity proofing failure to determine if IAL2 or IAL3 is the most appropriate selection. The primary identity proofing failure an agency may encounter is accepting a falsified identity as true, therefore providing a service or benefit to the wrong or ineligible person. In addition, proofing, when not required, or collecting more information than needed, is a risk in and of itself. Hence, obtaining verified attribute information when not needed is also considered an identity proofing failure. This step should identify if the agency answered Step 1 and 2 incorrectly, realizing they do not need personal information to deliver the service. Risk should be considered from the perspective of the organization and to the user, since one may not be negatively impacted while the other could be significantly harmed. The risk assessment process of M-04-04 and any agency specific risk management process should commence from this step.</td> 
  </tr>
  <tr>
    <td><img src="sp800-63-3/media/ial-step4.png" alt="IAL Step 4"/></td>
  </tr>
  <tr>
   <td>Step 4 is intended to determine if the personal information required by the agency will ultimately resolve to a unique identity. In other words, the agency needs to know the full identity of the subject accessing the digital service, and pseudonymous access, even with a few validated and verified attributes, is not possible. If the agency needs to uniquely identify the subject, the process can end. However, the agency should consider if Step 5 is of value to them, as the acceptance of claims will reduce exposure to the risk of over collecting and storing more personal information than is necessary.</td> 
  </tr>
  <tr>
    <td><img src="sp800-63-3/media/ial-step5.png" alt="IAL Step 5"/></td>
  </tr>
  <tr>
   <td>Step 5 focuses on whether the digital service can be provided without having access to full attribute values. This does not mean all attributes must be delivered as claims, but this step does ask the agency to look at each personal attribute they have determined they need, and identify which ones can suffice as claims and which ones need to be complete values. A federated environment is best suited for receiving claims, as the digital service provider is not in control of the attribute information to start with. If the application also performs all required identity proofing, claims may not make sense since full values are already under control of the digital service provider.</td> 
  </tr>
  <tr>
    <td><img src="sp800-63-3/media/ial-step6.png" alt="IAL Step 6"/></td>
  </tr>
  <tr>
   <td>If the agency has reached Step 6, claims should be used. This step identifies the digital service as an excellent candidate for accepting federated attribute claims from a CSP (or multiple CSPs), since it has been determined that complete attribute values are not needed to deliver the digital service.</td> 
  </tr>
  </table>
</div>



#### <a name="FAL_CYOA"></a> 5.3.4. Selecting FAL

All FALs require assertions to have a baseline of protections, including signatures, expirations, audience restrictions, and others enumerated in [[SP 800-63C]](sp800-63c.html#sec5). When taken together, these measures make it so that assertions cannot be created or modified by an unauthorized party, and that an RP will not accept an assertion created for a different system. 

RPs should use a back-channel [presentation mechanism](sp800-63c.html#sec6) where possible, as such mechanisms allow for greater privacy and security. Since the subscriber handles only an assertion reference and not the assertion itself, there is less chance of leakage of attributes or other sensitive information found in the assertion to the subscriber's browser or other programs. Since the assertion reference is presented by the RP directly to the IdP, the IdP can often take steps to identify and authenticate the RP during this step. Furthermore, since the assertion is fetched by the RP directly from the IdP over an authenticated protected channel, there are fewer opportunities for an attacker to inject an assertion into an RP.

FAL2 and higher require the assertion itself to be encrypted such that the intended RP is the only party that can decrypt it. This method not only improves the enforcement of audience restriction at RPs (since an unintended RP won't be able to decrypt an assertion), but also increases privacy protection by protecting the assertion message itself in addition to having it be passed along authenticated protected channels. RPs that allow front-channel presentation of assertions should require at least FAL2 to protect the content of the assertion, since the assertion can be seen by the subscriber and handled by the subscriber's browser.

FAL3 further requires that the subscriber prove possession of a key in addition to the ability to present an assertion or assertion reference. This method allows the RP to strongly verify the binding of the assertion to the subscriber by means of a key held by the subscriber. This key is referenced in the assertion and represents the subscriber.

Increasing the FAL increases the complexity of the deployment and management of a federation system, as RP keys need to be managed at FAL2 and FAL3 and subscriber keys additionally need to be managed at FAL3. Therefore, RPs should add advanced functionality where it is feasible and warranted for the application.

### <a name="toFedorNotToFed"></a> 5.4. Federation Considerations

The technical guidelines detailed in NIST SP 800-63-3 and its companion volumes are agnostic to the authentication and identity proofing architecture an agency selects. However, there are scenarios an agency may encounter which make identity federation potentially more attractive than establishing identity services local to the agency or individual applications. The following list details the scenarios where an agency may consider federation as a viable option. This list does not factor in any economic benefits or weaknesses of federation vs. localized identity architectures.

**Federate authenticators when:**

* Potential users already have an authenticator at or above required AAL.
* Multiple credential form factors are required to cover all possible user communities.
* Agency does not have infrastructure to support authentication management (e.g., account recovery, authenticator issuance, help desk).
* Desire to allow the primary authentication to be modified and upgraded over time without changing the RP's implementation.
* There are different environments to be supported, as federation protocols are network-based and allow for implementation on a wide variety of platforms and languages.
* Potential users come from multiple communities, each with its own existing identity infrastructure.

**Federate attributes when:**  

* Pseudonymity is required, necessary, feasible, or important to stakeholders accessing the service.
* Access to the service only requires a partial attribute list.
* Access to the service only requires at least one attribute claim.
* Agency is not the authoritative source or issuing source for required attributes.  
* Attributes are only required temporarily during use (such as to make an access decision), such that agency does not need to locally persist the data.



