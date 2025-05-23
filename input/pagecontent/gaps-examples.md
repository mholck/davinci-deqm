
###  Introduction

The Gaps in Care is a powerful report that when used along with other DEQM functionality can result in better care results for the payer, the provider, and most importantly the patient. A simplistic example described below shows its potential power.

In this scenario, a provider has requested a Gaps in Care Report for the Colorectal Cancer Screening Measure from ABC Insurance for their patients using the [care-gaps](OperationDefinition-care-gaps.html) operation. Since the provider does not specify [open, closed, or prospective gaps], all types of gaps will be returned as default. The Gaps in Care Report returns two patients. The first patient has an [open gap] and the second patient has a [closed gap].

The first patient with the [open gap] had a colonoscopy done recently and the ABC Insurance has not yet been made aware of. The provider uses the DEQM Submit Data functionality to update the payer’s database.

Later, the provider runs the same Gaps in Care Report again, both patients now showing as having their Colorectal Cancer Screening Measure gaps closed.

### Assumptions of Data Completeness
When patients change providers or healthcare payers, health data can become fragmented and isolated. One assumption of generating a Gaps in Care Report is that all relevant data to compute the gap has been collected, whether from previous payers or elsewhere. A payer-to-payer data exchange API, such as that specified in the [Da Vinci Payer Data Exchange (PDex) IG](https://www.hl7.org/fhir/us/davinci-pdex/index.html), provides great utility when calculating gaps in care. The ability to add a [Care Gap Remark](gaps-in-care-reporting.html#add-remark-to-gaps-in-care-report), which can indicate that some sort of evidence exists to close a gap in care, could be utilized as a trigger to employ a payer-to-payer API as needed.

### FHIR Resource Overview

#### Resources Supported for this Use Case
{:.no_toc}

|Resource Type| Profile Name                       | Link to Profile                      |
|---|------------------------------------|--------------------------------------|
|Bundle| DEQM Gaps In Care Bundle Profile   | [DEQM Gaps In Care Bundle Profile]   
|Composition| DEQM Gaps In Care Composition Profile | [DEQM Gaps In Care Composition Profile] 
|DetectedIssue| DEQM Gaps In Care DetectedIssue Profile | [DEQM Gaps In Care DetectedIssue Profile] 
|Encounter| QICore Encounter Profile           | [QICore Encounter]                   |
|Library| CRMI Shareable Library             | [CRMI Shareable Library]                       |
|Measure| CRMI Shareable Measure Profile     | [CRMI Shareable Measure]                       |
|MeasureReport| DEQM Individual MeasureReport Profile | [DEQM Individual MeasureReport Profile] |
|Organization| QICore Organization Profile        | [QICore Organization]                |
|Patient| QICore Patient Profile             | [QICore Patient]                     |
|Practitioner| QICore Practitioner Profile        | [QICore Practitioner]                |
|Procedure| QICore Procedure Profile           | [QICore Procedure]                   |

### Care Gaps Operation
{:.no_toc}
A Client, such as a provider, will use the [care-gaps](OperationDefinition-care-gaps.html) operation to request a Colorectal Cancer Screening measure Gaps in Care Report for their patients from the Server, such as a payer. The provider receives the report from the payer system and notices that the report identifies one of the patients as having an [open gap]. The provider looks at the chart and sees that a colonoscopy was done previously when the patient had a different payer/insurance. The provider then submits a DEQM Data Exchange MeasureReport and the referenced resources required as supporting evidence for the Colorectal Cancer Screening measure to the payer. Later, the provider repeats the [care-gaps](OperationDefinition-care-gaps.html) operation for Colorectal Cancer Screening to ensure the [open gaps] is now closed for that patient.

The Figure 3-22 shows the workflow for gaps in care.

{% include img-portrait.html img="gaps-swimlane-caregap-report.png" caption = "Figure 3-22 Gaps in Care Workflow" %}

### Gaps in Care Report
{:.no_toc}

This section contains an example that begins with a provider requesting a Gaps in Care Report for her patients from the payer. The provider receives the report, and she then submits additional data to the payer for the patient that was identified as having [open gaps] and [prospective gaps] in the report. She then later requests a Gaps in Care Report for these patients again from the payer.

#### Step 1 - Initial Run for a Gaps in Care Report
The resource graphs below represent the structure of the resources returned from the first [care-gaps](OperationDefinition-care-gaps.html) operation.

Figure 3-23 shows the patient, *Gaps Patient01*, has an [open gap] because there were no resources in the payer system that would put her in numerator or denominator exclusion of the Colorectal Cancer Screening measure. The DetectedIssue resource contained in this Gaps in Care Report has the code "open-gap" as gap status indicating *Gaps Patient01* has an [open gap] for this measure.

Figure 3-24 shows the second patient, *Gaps Patient02*, that has a [closed gap]. The `evaluatedResource` points to a colonoscopy procedure done in 2018 that had met the numerator criteria and resulted as a [closed gap]. Notice that the DetectedIssue resource's gap status code is "closed-gap", which indicates that *Gaps Patient02* does not have an [open gap] or a [prospective gap].

Figure 3-25 shows the third patient, *Gaps Patient03*, that has a [prospective gap]. The `evaluatedResource` points to a colonoscopy procedure done in 2010 that will be older than 10 years at the [gaps through period]. Because 10 years is the cutoff for a colonoscopy in the measure, the DetectedIssue resource's gap status code is "prospective-gap", which that a discrepancy will exist in the future between recommended best practices and the services that are actually provided and documented unless actions are taken.  

{% include img-portrait.html img="gic-colonoscopy-example-pt1-step1-open-gap.png" caption = "Figure 3-23 Gaps in Care Resources Colonoscopy Gaps Patient01 Example: Step 1 - Open Gap" %}

{% include img-portrait.html img="gic-colonoscopy-example-pt2-step1-no-gap.png" caption = "Figure 3-24 Gaps in Care Resources Colonoscopy Gaps Patient02 Example: Step 1- Closed Gap" %}

{% include img-portrait.html img="gic-colonoscopy-example-pt3-step1-prospective-gap.png" caption = "Figure 3-25 Gaps in Care Resources Colonoscopy Gaps Patient03 Example: Step 1- Prospective Gap" %}

{% include examplebutton.html example="get-gaps-bundle-initial-run-example" b_title = "Click Here To See Example of the Gaps in Care Report Described in Step 1" %}

#### Step 2 - Data Exchange to Submit Data for Closing Gaps

The provider noticed *Gaps Patient01* was indicated as having an [open gap] for Colorectal Cancer Screening. The provider ordered a colonoscopy and the patient was able to get it done in the next few days. Typically a claim by the colonoscopy performer will close the gap. However, the provider may elect to send the submit report data to close the gap. Please see [Colorectal Cancer Screening (COL)] Use Case for details on how to complete the DEQM Data Exchange.

The provider also noticed *Gaps Patient03* was indicated as having a [prospective gap] for Colorectal Cancer Screening. The colonoscopy from 10 years ago is only valid for the measure numerator until 2020-06-05. The provider ordered a colonoscopy and the patient was able to get it done in quickly. This ensures that the procpective gap is closed and does not become an open gap.

In some cases where there is a delay between order and performance, a provider may want to "soft close" the gap for some amount of time due to the existing order. This prevents the gap from showing up as "open" for some amount of time or until the order is carried out. The gaps in care remark extension allows for such functionality, and is described further in ([DEQM Parameters Care Gap Remark Patch Profile](StructureDefinition-parameters-careGap-Remark-Patch.html)).

([Parameters MeasureReport01 Patch Example](Parameters-measurereport01-patch.html))

([Parameters MeasureReport01 Patch Multiple Example](Parameters-measurereport01-patch-mult.html))

#### Step 3 - Rerun for a Gaps in Care Report

The provider rerun the Colorectal Cancer Screening Gaps in Care Report and confirmed that the [open gap] for *Gaps Patient01* and the [prospective gap] for *Gaps Patient03* were closed. Note that in the Figure 3-26 below, the DetectedIssue resource for *Gaps Patient01* now has the gap status code "closed-gap", because the [open gap] is now closed. The *Gaps Patient01* shows a recent colonoscopy. Figure 3-27 shows that there are no changes to *Gaps Patient02* comparing to the initially generated report, it still shows [closed gap]. Figure 3-28 shows that the [prospective gap] is now a [closed gap].

{% include img-portrait.html img="gic-colonoscopy-example-pt1-step3-gap-closed.png" caption = "Figure 3-26 Gaps in Care Resources Colonoscopy Gaps Patient01 Example: Step 3 - Open Gap Closed" %}

{% include img-portrait.html img="gic-colonoscopy-example-pt2-step1-no-gap.png" caption = "Figure 3-27 Gaps in Care Resources Colonoscopy Gaps Patient02 Example: Step 3 - Closed Gap" %}

{% include img-portrait.html img="gic-colonoscopy-example-pt3-step3-prospective-gap-closed.png" caption = "Figure 3-28 Gaps in Care Resources Colonoscopy Gaps Patient03 Example: Step 3 - Prospective Gap Closed" %}

{% include examplebutton.html example="get-gaps-bundle-rerun-example" b_title = "Click Here To See Example of the Gaps in Care Report Described in Step 3" %}

---

{% include link-list.md %}
