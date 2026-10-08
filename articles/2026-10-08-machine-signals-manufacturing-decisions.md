# From Machine Signals to Better Manufacturing Decisions

A production line reports that it ran for most of the shift, yet the number of shippable units is short. Was the line stopped too often, running slowly, or producing defects? A single headline such as “the plant was 90% productive” cannot answer that. Manufacturing teams need connected evidence: machine state, production counts, product history, and the context to interpret them.

That is the practical promise of smart manufacturing. In the selected section of *Architectural Patterns and Techniques for Developing IoT*, Jasbir Singh Dhaliwal presents manufacturing as a domain where real-time operational visibility, optimization, and automation can improve efficiency. The discussion links IoT to other technologies and concepts, including digital threads, operational technology, analytics, and human-machine interaction. The central engineering challenge is not merely collecting more data. It is connecting useful data across systems and turning it into decisions without losing sight of workers, product quality, or environmental effects.

## Smart manufacturing is a connected capability, not one gadget

Factories already use automation. Computer Numerical Control (CNC) machines, for example, carry out programmed operations such as milling or cutting. Smart manufacturing extends beyond automating an individual task. It connects production equipment and business systems so that information can be used across a product’s journey and, potentially, across plants and supply chains.

The section uses “smart manufacturing” as an umbrella term for this direction. It describes IoT as foundational, while identifying complementary technologies such as cloud computing, analytics, robotics, augmented and virtual reality, and additive manufacturing. These are not interchangeable parts in a shopping list. Each supports different work. Sensors can report machine conditions; analytics can help interpret trends; a cloud service can bring together data from multiple sites; and augmented reality can put instructions into a technician’s view of equipment.

An important boundary remains: connecting operational technology (OT) to information technology (IT) does not make them the same environment. OT monitors or controls physical processes through equipment such as programmable logic controllers, SCADA systems, and CNC machines. IT commonly handles business and information-processing workloads. Their networks, availability needs, and security risks differ. A production-line integration therefore needs deliberate security and operational review, not just a route to a dashboard.

## A digital thread gives data a life-cycle context

A sensor reading becomes more useful when it can be associated with the right machine, process, product, and point in time. A digital thread connects otherwise separate manufacturing systems and processes, providing an integrated view as a product moves from concept and production through use, service, repair, and decommissioning. The section describes sensors across production lines and connected products as one way to enable that continuity.

Imagine a pump with a persistent product identifier. Its manufacturing record links a motor test to the relevant production lot. After shipment, operating measurements can be associated with that same pump. If service staff later replace a component, the repair record can inform analysis of future product designs. This example illustrates the kind of link a digital thread can support; it does not imply that every sensor or record automatically shares a common identity or format. Those links require system design, consistent identifiers, and reliable data handling.

The potential benefits follow from that continuity. Teams can compare production lots with specifications, investigate service issues, reduce wasted material, and share information that might otherwise stay in separate systems. A manufacturer may also use connected-product information to offer services alongside a physical product. These are opportunities, not guaranteed outcomes: their value depends on data quality, safe operations, and a real need for the resulting service.

## Read OEE as three different questions

Overall Equipment Effectiveness (OEE) is a way to describe production performance using availability, performance, and quality:

$$
\mathrm{OEE} = A \times P \times Q
$$

The section defines availability, $A$, as actual production time divided by planned production time. Performance, $P$, is actual work speed divided by planned work speed. It defines quality, $Q$, as the number of actual units produced divided by the total number planned. OEE can be measured at different levels, such as an assembly line, department, or plant, but comparisons only make sense when the measurement boundaries and definitions are aligned.

Here is a worked example using those definitions. A line is planned to produce for 10 hours at 60 units per hour, so its planned quantity is $10 \times 60 = 600$ units. It runs for 9 hours, produces 450 units, and its observed speed during production is 50 units per hour.

Availability is $9/10 = 0.90$. Performance is $50/60 = 5/6 \approx 0.8333$. Using the section’s stated quality definition, quality is $450/600 = 0.75$. Therefore:

$$
\mathrm{OEE} = 0.90 \times \frac{5}{6} \times 0.75 = 0.5625 = 56.25\%
$$

The arithmetic is consistent: 9 hours at 50 units per hour gives 450 units. The result is a descriptive metric under the definitions and boundary chosen. Notice a limitation in the stated quality ratio: actual units produced divided by planned units does not distinguish good units from defective units. If a plant intends quality to mean the share of output meeting specification, it needs a separate, explicitly agreed numerator and denominator. Do not silently substitute a different convention when comparing results.

## Manufacturing has more than one production shape

Discrete manufacturing produces countable items or parts, such as furniture, machine components, or phones. Individual units can potentially be tracked through their life cycles when they have suitable identifiers and connectivity. Process manufacturing transforms ingredients or raw materials using recipes and physical or chemical processes, as in paint or petrochemical production. Its inputs and outputs may not be uniquely countable in the same way as individual products.

This difference affects data models. A discrete-production record may identify one unit or lot. A process record may need to describe a batch, recipe, quantities, and conditions. Treating both as identical “items” can obscure how production actually works. The broader lesson is to model the production process before choosing a tracking scheme or metrics.

Manufacturing execution systems (MES) coordinate and track execution of production processes, including tasks and schedules. SCADA systems monitor and control industrial processes, commonly offering an operator interface for real-time or near-real-time visualization and action. These capabilities can overlap with smart manufacturing. The section positions smart manufacturing as broader in its use of analytics, connections to enterprise systems such as ERP and supply-chain management, and newer technologies. In practice, teams should map existing responsibilities before replacing or connecting systems. A new data platform should not accidentally become an untested control path.

## Progress is not only a faster factory

The section traces industrial change from mechanization and steam, through electrical power, and then electronics and computer-based automation. It describes Industry 4.0 in terms of technologies including sensors, robotics, AI, and cloud services. It then presents Industry 5.0 as a proposed shift toward human-centric work, sustainability, resilience, and collaboration between people and machines.

That distinction matters when setting project goals. A system that increases throughput but makes work less safe or creates avoidable waste is not an unqualified improvement. Connected monitoring might help detect a hazardous condition or support maintenance. Augmented reality can present repair information alongside equipment, while virtual reality can support simulated training. Additive manufacturing can help create prototypes or customized products, although its suitability depends on the material, product, and process. These are tools for particular needs, not automatic benefits of digitization.

A sound project begins with a measurable operational question: Which downtime events need investigation? Can a production lot be linked to the specification it was meant to meet? Is there a service problem that product-use data could help resolve? Teams then identify the relevant signals, their owners, their security requirements, and the decision each signal is meant to support. This approach keeps the work grounded in a real production or research workflow rather than in data collection for its own sake.

## Pitfalls to catch before deployment

First, define units and time windows. “Production time” may exclude planned breaks, changeovers, or maintenance, depending on local rules. Without a shared definition, a comparison between plants can be mathematically correct and operationally misleading.

Second, preserve context and identity. A temperature without a unit, timestamp, equipment identifier, or calibration history may be difficult to interpret. A digital thread depends on meaningful links across systems, not merely on storing more measurements.

Third, keep analysis separate from control until it has been validated. A reporting system and a system that changes equipment behavior have different risk profiles. OT security, safe failure behavior, and operator review deserve explicit attention.

Finally, treat metrics as prompts for investigation, not as complete explanations. OEE can help locate a performance concern, but it does not by itself explain why the concern occurred or establish that one intervention caused a change. Pair it with operational context and appropriate review.

## Exercises, with short answers

1. A line has 400 minutes of planned production and 360 minutes of actual production. What is availability? **Answer:** $360/400 = 0.90$, or 90%.

2. Why might a product identifier matter to a digital thread? **Answer:** It can help link manufacturing, service, and use records to the same product, provided the systems maintain consistent identity and context.

3. A plant reports OEE values from two sites. What should you check before comparing them? **Answer:** Confirm that the sites use the same time boundaries, production-speed basis, quality definition, and measurement scope.

Smart manufacturing is best understood as a way to connect operational evidence across processes and product life cycles. IoT supplies important signals, but useful outcomes depend on the surrounding systems, careful definitions, secure integration, and human judgment. The factory becomes smarter not when every machine is online, but when the right information can support a better, safer, and more responsible decision.

---

Published 2026-10-08.

**Source:** Architectural Patterns and Techniques for Developing IoT -- Jasbir Singh Dhaliwal, section **7 Pattern Implementation in the Manufacturing Domain**.

This is an original explanatory note; the source book is not redistributed.
