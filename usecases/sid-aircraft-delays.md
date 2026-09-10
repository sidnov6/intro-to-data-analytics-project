# Predicting Aircraft Delays

| Field | Value |
|---|---|
| Author | sidnov6 |
| Date | 2026-09-10 |
| Status | `ready-for-review` |

[Download the Word proposal](assets/aircraft-delay-proposal.docx)

*Project proposal for Intro to Data Analytics*

We propose studying how a late arrival affects the next flight using the same aircraft. Using public flight records, we will predict which departures are at risk and explore where extra time between flights could make a schedule more reliable.

## The problem we want to solve

An airline uses the same aircraft for several flights. After landing, the plane needs time for passengers to leave, cleaning, refueling and boarding before it can depart again. This period is called turnaround time. If one flight arrives late, the next flight has less time to get ready. The delay can then spread through the aircraft’s remaining flights.

This is a supply chain problem because one operation depends on a resource becoming available from the previous operation. Here, the shared resource is the aircraft. Delays disrupt that flow, leaving passengers waiting and forcing staff to adjust their plans.

## A real example from the data

On 1 January 2025, a Southwest aircraft flying from Las Vegas to Kansas City was scheduled to arrive at 3:10 pm. It arrived at 3:40 pm. Its next flight, to St. Louis, was scheduled to leave at 4:05 pm.

The original schedule allowed 55 minutes on the ground. After the late arrival, only 25 minutes remained. The next flight actually left at 4:23 pm, an 18-minute delay.

At 3:40 pm, could we have identified that next departure as a flight needing attention?

## Who would use the result

The intended user is an airline operations coordinator monitoring upcoming departures. A useful output would be a list of flights ranked by their estimated chance of leaving late, alongside the time available before departure. Staff could use it to decide which flights to review first, check readiness with ground teams and prepare passenger updates.

The business question is whether this ranking would identify more problem flights than a simple rule, such as checking the aircraft with the least time left before departure. We will test the quality of that ranking; any improvement in airline operations would need a separate operational trial.

## The data we can use

Our main source is the US Bureau of Transportation Statistics dataset called Reporting Carrier On-Time Performance. It contains scheduled and actual flight times, routes, aircraft registration numbers and delay information. We propose using Southwest’s reported domestic flights for 2025 to keep the project focused.

The data is free to download in monthly files, without an API key. All twelve 2025 archives were accessible during the feasibility check and total about 355 MB compressed. January, July and November were downloaded and inspected. Those months alone produced 230,157 usable aircraft connections where the incoming plane arrived at least 15 minutes before the next scheduled departure. Of these, 44,132 departed at least 15 minutes late.

| Data we need | How we will use it |
| --- | --- |
| Aircraft registration number | Connect an incoming flight to the next observed flight using the same plane. |
| Scheduled and actual arrival times | Measure the incoming flight’s delay and establish when we make the prediction. |
| Next flight’s schedule and route | Calculate the time available before departure and identify its airport and destination. |
| Date and departure hour | Account for differences between seasons, weekdays and times of day. |
| Actual departure time | Check whether the next flight was late and by how many minutes. This is the answer the model must predict. |

We will also use OpenFlights airport time-zone information to put timestamps on a consistent clock. We will remove broken aircraft links and inconsistent times, and report canceled and diverted flights separately. The main model will therefore describe completed flights with usable aircraft connections.

## The models we will compare

Our main task is classification: estimate the probability that the next flight will depart at least 15 minutes late. Each example represents an incoming flight and its next observed departure. We will only give the model information available when the incoming aircraft reaches the gate.

| Approach | Role in the project |
| --- | --- |
| Simple baseline | Use past delay rates and the time remaining before departure. This shows what we can achieve with straightforward rules. |
| Logistic regression | Our first model. It estimates delay probability from factors such as incoming delay, turnaround allowance, airport and departure hour. |
| Random forest classifier | Our comparison model. It can capture combinations of conditions, such as a short turnaround being more risky at particular airports or times. |
| Ridge regression (optional extension) | Predict the number of delay minutes. Ridge is a form of linear regression that limits overly large coefficients. |

## How we will judge whether it works

We will train on earlier months and test on later ones, so the model must predict flights it has not seen. A proposed split is January to August for training, September to November for choosing the model, and December for the final test.

The clearest business measure is how many delayed departures appear among the flights flagged for review. For example, if staff could review only the highest-risk 10% of flights, would our model catch more late departures than the simple baseline? We will also count false alarms, because unnecessary checks take staff time.

We will check whether predicted probabilities are believable. Among flights assigned a 30% delay risk, roughly 30% should actually be late. For the optional regression model, we will measure the average prediction error in minutes.

Accuracy alone would be misleading: about 81% of connections in the inspected prediction sample departed less than 15 minutes late. A model that always predicted that outcome would already look accurate. Our model must do better at identifying the departures that need attention.

## The schedule planning extension

Airlines can allow extra time between flights to absorb disruption. This is called a schedule buffer. Extra time has a cost: aircraft spend longer waiting, and the airline may fit fewer flights into the day. The planning question is where a limited amount of extra time would be most useful.

We propose a small simulation that compares spreading the same total amount of extra time evenly across connections with concentrating it at connections that historical data suggests are more vulnerable. Each approach will use the same budget and the same simulated disruptions. Any planning model must use information available before the day’s flights, rather than their eventual arrival delays.

The simulation will need assumptions about the time required to turn an aircraft around, because the public data does not record when every ground task finishes. We will test several assumptions and report whether the preferred allocation changes. We will also check actual completion times, since moving a published departure later can make a flight look punctual without making it leave any earlier.

## What the project will deliver

The main output will be a documented Jupyter notebook showing how we connect flights, clean the records, compare models and evaluate their predictions. A small results dashboard or summary table will show which departures the model flags and how often those warnings are correct. The optional simulation will show the trade-off between extra schedule time and reliability under its stated assumptions.

The main limitation is that the records show which aircraft actually operated each flight, but do not establish what assignments staff knew in advance. This makes the project a historical study of observed aircraft connections. We can test prediction quality and explore planning scenarios; we cannot promise that a buffer change would prevent a particular real delay or produce a verified financial saving.

## Data sources and supporting material

[BTS flight dataset and field definitions](https://www.transtats.bts.gov/Fields.asp?gnoyr_VQ=FGJ)

[BTS monthly download example for January 2025](https://www.transtats.bts.gov/PREZIP/On_Time_Reporting_Carrier_On_Time_Performance_1987_present_2025_1.zip)

[OpenFlights airport data and time-zone documentation](https://openflights.org/data.php)

Sample counts come from the project’s data feasibility audit dated 10 September 2026. Availability has been checked; model performance has not yet been tested.

## Discussion notes

**Research question:** At inbound gate arrival, can a model identify the next observed departure that will leave at least 15 minutes late more effectively than a simple rule based on the time remaining?

The smallest useful version is a cleaned table of Southwest aircraft connections, a baseline, logistic regression and evaluation on later months. Random forest is the comparison model; predicting delay minutes and simulating schedule buffers are optional extensions. The aircraft connection, rather than a passenger or an individual airport, is the unit of analysis.

The downloaded sample and tested joins establish data feasibility. Full-year cleaning and model evaluation remain to be done. The three audited months are not a continuous rotation history and must not be linked across missing months. Calendar dates share weather and operational disruption, so evaluation should also report variation across dates.

### Access and practical limits

- BTS monthly files are publicly downloadable. Keep source attribution and document any revisions to the downloaded files.
- OpenFlights publishes its airport database under the Open Database License. Retain attribution and check the redistribution terms when sharing derived airport data.
- The proposed inputs contain flight operations, not passenger records. Aircraft registration numbers are used to reconstruct connections.
- Incomplete aircraft histories, canceled flights and final aircraft assignments limit the conclusions. The model does not cover every disruption an airline would face in live use.
- Computation is manageable for a small model; reconstructing timestamps and avoiding future information are the main engineering tasks.

### Questions for the team

- Does the group want to shortlist this proposal for discussion?
- Should the first version stop at classification, with the schedule simulation attempted only after the prediction results are complete?

This proposal is ready for review. The team has not selected it as the course project.
