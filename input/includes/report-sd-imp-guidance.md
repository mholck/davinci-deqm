- The `MeasureReport.group.population.id` is a must support element because it is used to link across the Measure and is equal to the `Measure.group.population.id`. This is needed because the `MeasureReport.group.population.code` is insufficient when the measure types have multiple populations with the same code.

- If the measure scoring type is 'proportion', 'ratio', or 'continuous-variable' then the `improvementNotation` element is required.

- If the measure scoring type is 'proportion' then the `MeasureReport.group.measureScore.value` SHALL be a numerical value between 0 and 1.

- The `reporter` should be consistent with the [X-Provenance header data]({{site.data.fhir.path}}provenance.html#header) if present.

- the DEQM Reporting Vendor Extension is intended to represent the submitting entity when it is not the same as the reporting entity.

- There are some elements like scoring and improvementNotation that can be specified either at the measure level or at the group level. If it is reported at the measure level it should not be specified for any group and if it is specified for any group it should be specified for every group and not at the measure level.
