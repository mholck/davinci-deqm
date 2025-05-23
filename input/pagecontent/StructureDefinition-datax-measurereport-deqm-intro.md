{% assign id = {{include.id}} %}
<!--Begin Generated Intro Tag (DO NOT REMOVE)-->
### Mandatory Data Elements and Terminology
The following data-elements are mandatory (i.e data MUST be present).

**Each {{site.data.structuredefinitions.[id].type}} Must Have:**
1. period.start: Starting time with inclusive boundary
2. subject: What individual(s) the report is for
3. status: complete \| pending \| error
4. type: individual \| subject-list \| summary \| data-collection
5. reporter: The organization where the measure was completed
6. period.end: End time with inclusive boundary, if not ongoing
7. updatetype: Optional Extensions Element
8. date: When the report was generated
9. period: What period the report covers
10. measure: What measure and version was calculated

**Each {{site.data.structuredefinitions.[id].type}} Must Support:**
1. evaluatedResource: What data was used to calculate the measure score
2. measurereport-category: What category is this measure report
3. description: Description of the population

<!--End Generated Intro (DO NOT REMOVE)-->



**Additional Profile specific implementation guidance:**

- *For a detailed discussion of incremental and snapshot updates see these sections on [Submit Data](datax.html#submit-updates) and [Collect Data](datax.html#collect-updates)

- The Producer may not have all the data that is required to calculate the measure report at that time it is transmitted. It that case the MeasureReport may not have *any* evaluatedResources (in other words, no measure data).  The missing data may be transmitted in a subsequent update or the additional data used in the measure is owned by an aggregator (such as a continuous coverage period requirement).

- The `reporter` should be consistent with the [X-Provenance header data]({{site.data.fhir.path}}provenance.html#header) if present.

A data producer

### Examples

{% include datax-measurereports.md %}

{% include link-list.md %}
