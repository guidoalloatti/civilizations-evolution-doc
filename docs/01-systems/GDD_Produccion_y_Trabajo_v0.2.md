> **Document control:** 0.2 | Status: conceptual design; decisions marked as confirmed are preserved from the source. **Scope:** this GDD is the detail source for its domain; the Master GDD establishes the shared vision. Cross-system conflicts are recorded in AUDIT_REPORT.md and are not resolved implicitly.

---

# GDD — PRODUCTION AND LABOR SYSTEM

**Version:** 0.2  
**Status:** Conceptual design  
**Historical period:** Paleolithic – approximately 1800 AD  
**Related systems:** Economy, population, construction, technology, logistics, religion, culture and politics.

---

# 1. GENERAL OVERVIEW

The production and labor system will determine how a civilization transforms available resources into goods, products and services.

Its goal is to represent the evolution of productive activities, from small communities of hunters and gatherers to highly specialized societies with industries, manufacturing, skilled workers and complex production systems.

The system must allow the player to:

- Build productive buildings freely.
- Organize their production chains.
- Make use of the resources available in the territory.
- Develop different economic specializations.
- Improve productivity through technology.
- Manage worker availability.
- Influence working conditions.
- Automate processes.
- Develop different economies depending on their decisions.
- Observe how their buildings evolve visually and functionally across the centuries.

Production must not be limited exclusively by technological unlocks. Technology will enable new possibilities and improve existing ones, but the environment, resources, social organization and the player's decisions will also be decisive.

## 1.1. Design principles

1. **Coherent production:** buildings produce goods in accordance with their capabilities and available resources.
2. **Freedom of construction:** the player organizes their settlements without depending on a predetermined layout.
3. **Specialization:** workers develop skills and professions.
4. **Automation:** processes run without constant intervention.
5. **Interdependence:** buildings form connected production chains.
6. **Historical evolution:** production methods change with technology and society.
7. **Consequences:** decisions about production and labor have economic and social effects.
8. **Configurable complexity:** the player chooses between simple or detailed management.
9. **Visual evolution:** buildings physically reflect the development of the civilization.
10. **Freedom of development:** not all civilizations have to evolve in the same way.

---

# 2. GENERAL PRODUCTION MODEL

Production will be organized around four components:

1. Buildings and facilities.
2. Workers.
3. Resources and inputs.
4. Production processes.

A productive building needs certain conditions in order to operate.

For example, a bakery needs:

- A suitable building.
- Workers.
- Flour.
- Water.
- Fuel, depending on the technology used.
- Access to inputs.
- Storage or distribution capacity for the finished product.

When these conditions are met, the building begins to produce.

Production will be limited by the most restrictive factor.

A bakery may have the capacity to produce 100 loaves per day, but if it only receives enough flour to produce 40, its effective output will be 40.

The player will be able to intervene to improve production conditions, but should not have to manually administer every operation.

---

# 3. PRODUCTIVE BUILDINGS

Buildings will be the main units of production.

Each building will have its own characteristics that determine what it can produce, what it needs and how it operates.

## 3.1. Types of buildings

### Extraction

Obtain resources directly from the environment.

Examples:
- Gathering camps.
- Hunting grounds.
- Fisheries.
- Quarries.
- Mines.
- Logging.
- Wells.

### Primary production

Transform natural resources through agricultural or livestock activities.

Examples:
- Farms.
- Orchards.
- Pastures.
- Breeding farms.
- Plantations.
- Fish farms.

### Transformation

Process raw materials and produce intermediate or final goods.

Examples:
- Mills.
- Bakeries.
- Sawmills.
- Pottery kilns.
- Smithies.
- Tanneries.
- Weaving workshops.

### Manufacturing

Produce more complex and specialized goods.

Examples:
- Tool workshops.
- Carpentry workshops.
- Shipyards.
- Textile factories.
- Furniture workshops.
- Machinery manufacturers.

### Services

Provide activities necessary for the functioning of society.

Examples:
- Markets.
- Schools.
- Administrative centers.
- Hospitals.
- Repair workshops.
- Transport services.

The emergence of each category will depend on the historical, technological and social conditions of each civilization.

## 3.2. Properties of each building

Each building will have a production profile with:

| Property | Description |
|---|---|
| Capacity | Maximum possible output |
| Workers | Required and maximum number |
| Specialization | Required professions |
| Inputs | Necessary resources |
| Products | Goods generated |
| Technology | Requirements and improvements |
| Efficiency | Process performance |
| Maintenance | Costs to keep it operational |
| Storage | Capacity to hold inputs and products |
| Operating radius | Area of influence, where applicable |
| Status | Operation, deterioration or inactivity |
| Ownership | Who controls the building and its profits |
| Evolution | Available improvements and transformations |
| Expansions | Modules and sectors that can be added |

---

# 4. PRODUCTION PROCESSES AND RECIPES

Each productive activity will be defined by a process.

A process establishes what resources are needed, how long it takes to produce and what results are obtained.

## 4.1. Structure of a recipe

Each recipe will have:

- Name.
- Required building.
- Inputs.
- Quantity of each input.
- Production time.
- Required workers.
- Required specialization.
- Required technology.
- Products generated.
- By-products, where they exist.
- Energy consumption.
- Wear on tools or machinery.

Example:

### Flour production

**Building:** Mill.

**Input:**
- 10 units of wheat.

**Process:**
- Milling.
- Consumption of mechanical energy.
- Processing time.

**Product:**
- 8 units of flour.

**By-product:**
- Usable remains for animal feed, depending on the process.

The values are illustrative and will need to be balanced during development.

## 4.2. Alternative recipes

The same product may be obtained through different methods.

For example, a civilization may produce flour through:

- Manual milling.
- Animal-powered mills.
- Water mills.
- Windmills.

Each method will have different requirements, costs and productivity.

This will allow technology not to be limited to unlocking new buildings, but also to transform existing processes.

## 4.3. Production flexibility

The system must allow new recipes to be added without needing to create a completely new building for each variant.

The same workshop may have different specializations or processes.

For example, a smithy may produce:

- Agricultural tools.
- Construction tools.
- Weapons.
- Metal parts.
- Decorative elements.

The player will set priorities, and the building will adapt its production to needs and demand, taking into account its resources and capabilities.

---

# 5. PRODUCTION CHAINS

Production chains will connect different buildings.

Their goal is to generate economic dependence between activities and to make territorial organization matter.

## 5.1. Example: food chain

1. The farm produces wheat.
2. Transport carries the wheat to the mill.
3. The mill transforms wheat into flour.
4. Transport carries flour to the bakery.
5. The bakery produces bread.
6. The bread is distributed to markets or points of consumption.
7. The population consumes the product.

Each stage depends on the previous one.

An interruption at the farm, in transport or at the mill can affect the entire chain.

## 5.2. Example: construction chain

1. The woodcutter obtains wood.
2. The quarry obtains stone.
3. The sawmill processes wood.
4. The workshop produces tools.
5. Transport distributes the materials.
6. Builders use the resources to erect buildings.

## 5.3. Production dependencies

Buildings may depend on multiple suppliers.

A smithy, for example, may need:

- Iron ore.
- Fuel.
- Auxiliary tools.
- Specialized workers.

The system must identify supply problems and tell the player where the bottleneck is.

## 5.4. Regional specialization

Production chains will not have to be concentrated in a single settlement.

The player will be able to develop:

- Agricultural regions.
- Mining regions.
- Manufacturing centers.
- Commercial cities.
- Specialized ports.

This will encourage trade and interdependence between settlements.

---

# 6. PRODUCTIVITY AND EFFICIENCY

Productivity will determine how much a building can produce under real conditions.

Not all buildings will have the same performance, even if they are of the same type.

## 6.1. Productivity factors

Effective output will depend on:

- Number of workers.
- Experience and skills.
- Technology.
- Quality of tools.
- Availability of raw materials.
- Quality of inputs.
- Condition of the building.
- Environmental conditions.
- Work organization.
- Accessibility and logistics.
- Working conditions.

## 6.2. Conceptual model

Productivity may be represented by a general formula:

**Effective output = Base capacity × Efficiency modifiers × Input availability.**

The modifiers may include:

- Labor efficiency.
- Technological efficiency.
- Material quality.
- Environmental conditions.
- Condition of the facilities.

Output must never exceed the physical capacity of the building or the process, unless a specific technology allows it to be modified.

## 6.3. Example

A farm has a base capacity of 100 units of wheat per season.

Its conditions are:

- Workers: 90% efficiency.
- Technology: 110%.
- Soil: 120%.
- Tool condition: 90%.

Final output will depend on the combination of these factors.

The values will need to be balanced to prevent small improvements from producing disproportionate increases.

## 6.4. Productivity and specialization

An experienced worker will be able to produce more than an untrained one.

Specialization will allow:

- Reduced production times.
- Improved quality.
- Reduced waste.
- The use of more complex processes.
- The development of new techniques.

Experience will be acquired through work, learning and training.

---

# 7. LABOR SYSTEM AND WORKER ASSIGNMENT

**Adopted decision: automatic assignment with player intervention.**

The player will not have to manually assign each citizen.

Inhabitants will seek job opportunities according to their capabilities, needs and preferences.

## 7.1. Automatic operation

When a productive building is constructed, it will generate jobs.

The system will look for available workers taking into account:

1. Distance between dwelling and workplace.
2. Required skills.
3. Experience.
4. Wages or benefits.
5. Working conditions.
6. Availability of workers.
7. The building's economic priority.

Assignment will be dynamic.

A worker may change jobs if they find a more convenient opportunity, provided social and economic conditions allow it.

## 7.2. Player intervention

The player will be able to:

- Set production priorities.
- Limit the number of workers.
- Define wages or benefits in advanced stages.
- Favor certain sectors.
- Set schedules or shifts where appropriate.
- Apply labor restrictions.
- Encourage the training of professionals.

The goal is to offer strategic control without forcing the player to individually manage each job.

## 7.3. Production priorities

Each building may have a priority:

- Critical.
- High.
- Normal.
- Low.
- Disabled.

If there is a shortage of workers, the system will use these priorities to distribute the labor force.

For example, during a food crisis, the player may prioritize farms and bakeries over furniture workshops.

## 7.4. Unemployment

The available population will not always find work.

Unemployment may produce:

- Lower income.
- Lower consumption.
- Discontent.
- Migration.
- Social problems.

However, a certain amount of unemployed population may also be necessary to maintain economic flexibility and to cover new activities.

---

# 8. PROFESSIONS AND SKILLS

Workers will be able to develop different professions and levels of specialization.

**Adopted decision:** each worker will have a primary profession and may develop secondary skills.

## 8.1. Professions

Professions will evolve along with society.

Examples:

**Primitive societies**
- Gatherer.
- Hunter.
- Fisher.
- Artisan.

**Agricultural societies**
- Farmer.
- Herder.
- Builder.
- Potter.

**Metallurgical societies**
- Miner.
- Smelter.
- Blacksmith.
- Specialized carpenter.

**Urban societies**
- Merchant.
- Administrator.
- Scribe.
- Architect.
- Physician.

**Modern societies**
- Engineer.
- Technician.
- Specialized operator.
- Accountant.
- Researcher.

## 8.2. Skill levels

Each profession may have different levels:

1. No experience.
2. Apprentice.
3. Competent worker.
4. Specialist.
5. Master.

The levels will not necessarily be identical for all professions.

## 8.3. Secondary skills

A worker may have a primary profession and additional knowledge.

Example:

**Juan**
- Primary profession: blacksmith.
- Secondary skill: carpentry.
- Experience: advanced in metallurgy.
- Experience: basic in construction.

This will allow some flexibility without turning every inhabitant into a universal worker.

## 8.4. Learning

Inhabitants will be able to acquire skills through:

- Work experience.
- Family learning.
- Training with other workers.
- Educational institutions.
- Guilds.
- Specialized training.

The transmission of knowledge will be important for technological evolution.

## 8.5. Labor mobility

Workers will be able to change professions.

However, some activities will require training and learning time.

This will prevent a civilization from instantly transforming its entire population into specialists in any sector.

---

# 9. WAGES, INCENTIVES AND WORKING CONDITIONS

Work will have an economic and social dimension.

Not all societies will operate under the same labor system.

## 9.1. Evolution of labor relations

Different models may exist:

- Communal labor.
- Family labor.
- Forced labor.
- Serfdom.
- Slave labor, where historically appropriate.
- Wage labor.
- Independent work.
- Entrepreneurial work.

Not all civilizations will have to go through the same stages or adopt the same models.

## 9.2. Wages

In societies with monetary economies, workers may receive wages.

These will depend on:

- Profession.
- Specialization.
- Labor supply and demand.
- Working conditions.
- Profitability of the activity.
- Legislation.
- Bargaining power.

## 9.3. Working conditions

The player will be able to influence:

- Working hours.
- Rest periods.
- Safety.
- Worker nutrition.
- Housing.
- Remuneration.
- Benefits.

Better conditions may increase satisfaction and productivity, but also raise costs.

Poor conditions may cause accidents, illness, conflict and loss of workers.

## 9.4. Labor conflicts

In more complex societies the following may appear:

- Protests.
- Wage demands.
- Strikes.
- Worker organizations.
- Guilds.
- Conflicts between owners and employees.

These occurrences will be connected to the political and social systems.

---

# 10. TECHNOLOGICAL EVOLUTION OF PRODUCTION

Technology will profoundly transform the way production is carried out.

It will not be limited to increasing numbers: it will enable new methods, buildings, tools and organizations.

## 10.1. Evolution of production methods

### Paleolithic

- Manual production.
- Rudimentary tools.
- Hunting and gathering.
- Communal production.
- Low specialization.

### Neolithic

- Agriculture.
- Livestock farming.
- Pottery.
- Agricultural tools.
- Storage of surpluses.
- Initial specialization.

### Bronze Age and Iron Age

- Metallurgy.
- More efficient tools.
- Specialized workshops.
- Artisanal production.
- Greater division of labor.

### Antiquity

- Mills.
- Irrigation canals.
- Specialized kilns.
- Large agricultural estates.
- Urban workshops.
- Productive infrastructure.

### Middle Ages

- Water mills.
- Windmills.
- Guilds.
- Specialized workshops.
- Advanced construction techniques.
- Greater productive organization.

### Early Modern period

- Manufacturing.
- Task specialization.
- Better tools.
- New agricultural techniques.
- Greater production scale.
- First forms of mechanization.

### Pre-industrial period

- Machines.
- More complex production systems.
- Advanced division of labor.
- Initial mechanization.
- Greater industrial concentration.

These stages are references, not a mandatory technology tree.

## 10.2. New production methods

A technology may enable different ways of carrying out an activity.

For example:

A mill may initially operate on human or animal power and later use hydraulic or wind energy.

This will allow buildings to maintain historical continuity.

## 10.3. Technological improvements

Technologies may modify:

- Production capacity.
- Production speed.
- Efficiency.
- Quality.
- Resource consumption.
- Worker requirements.
- Production methods.
- Appearance of the facilities.

---

# 11. VISUAL AND FUNCTIONAL EVOLUTION OF BUILDINGS

**This is one of the core identity mechanics of the game.**

Buildings will not be static structures that are simply unlocked and replaced.

Each structure may evolve through improvements, expansions and transformations enabled by different occurrences.

The goal is for the player to be able to observe how a small primitive village gradually transforms into a complex civilization.

## 11.1. Evolution triggers

A new evolution may be enabled for different reasons:

### Technological
- Discovery of new techniques.
- New tools.
- New materials.
- New construction methods.
- New sources of energy.

### Historical
- Transition to a new age.
- Changes in historical conditions.
- Development of new institutions.

### Religious
- Emergence of a new religion.
- Religious reforms.
- Changes in religious practices.
- New ceremonial or institutional needs.

### Cultural
- New customs.
- New knowledge.
- New forms of organization.
- Aesthetic and architectural changes.

### Social
- Emergence of new social classes.
- New needs of the population.
- Changes in labor relations.
- Development of institutions.

### Political
- Changes of government.
- New laws.
- Administrative reforms.
- Changes in territorial organization.

### Economic
- New forms of ownership.
- Development of trade.
- Production specialization.
- New market needs.

Triggers will not be exclusively technological.

A religious or cultural change may also modify the architecture, functions and needs of certain buildings.

## 11.2. Improvement notification system

When a building has a new evolution available, the player will receive a visible indication.

The option will appear:

- On the building itself.
- In its representation on the map.
- In the building's information panel.

The interface may display:

**New evolution available: Medieval smithy.**

Requirements met:
- Advanced metallurgical technology.
- Availability of iron.
- Specialized knowledge.

Benefits:
- Greater production capacity.
- New recipes.
- Better tool quality.
- New architectural appearance.

The player will be able to review the changes and decide when to apply the improvement.

## 11.3. Building transformations

Evolutions may modify:

- Exterior architecture.
- Construction materials.
- Equipment.
- Tools.
- Machinery.
- Production capacity.
- Required professions.
- Available products.
- Infrastructure needs.
- Functions of the building.

An improvement will not necessarily be a simple increase in productivity.

It may represent a profound change in the way an activity operates.

## 11.4. Historical visual evolution

Buildings must reflect the historical and cultural context of the civilization.

For example:

A smithy may evolve from a small artisanal workshop into a production complex with different sectors, better furnaces, storehouses and specialized workers.

A farm may evolve from a simple family holding into a more organized agricultural facility, with better tools, irrigation systems and storage.

The evolution will not be identical across all civilizations.

Geography, available materials, culture, religion and technologies will influence its appearance.

## 11.5. Non-mandatory evolution

The player will not be obliged to apply every improvement.

They may keep old buildings when it makes economic, cultural or strategic sense.

A historical structure may continue to operate if it has the necessary resources and conditions.

This will make it possible to create cities with different historical and architectural layers.

---

# 12. PHYSICAL AND MODULAR EXPANSION OF BUILDINGS

**Adopted decision: expansion through modules and sectors.**

Buildings will be able to grow physically as the civilization develops.

Not all of them will have to be replaced by completely new versions.

## 12.1. Modular system

A building may incorporate different modules.

Examples:

**Smithy**
- Main workshop.
- Ore storehouse.
- Fuel storehouse.
- Smelting area.
- Fabrication sector.
- Storage area.

**Farm**
- Family dwelling.
- Granary.
- Stable.
- Tool storehouse.
- Processing area.
- Irrigation facilities.

**Sawmill**
- Wood reception area.
- Processing sector.
- Storehouse.
- Drying area.
- Complementary workshop.

## 12.2. Benefits of expansions

Modules may:

- Increase capacity.
- Improve efficiency.
- Incorporate new recipes.
- Reduce logistics costs.
- Allow more workers.
- Incorporate new technologies.
- Diversify production.

## 12.3. Restrictions

Expansions will depend on:

- Available space.
- Resources.
- Technology.
- Costs.
- Labor.
- Geographic conditions.

Physical expansion must respect the player's freedom of construction.

---

# 13. QUALITY AND VARIETY OF PRODUCTS

**Adopted decision: gradual introduction of quality.**

Not all products will have to be identical.

In advanced stages, quality may become an important economic factor.

## 13.1. Quality factors

Quality will depend on:

- Raw materials.
- Worker experience.
- Technology.
- Tools.
- Production time.
- Process conditions.

## 13.2. Historical evolution

In the earliest eras, quality will have a limited influence and will be simplified.

As specialized professions, manufacturing and more complex markets appear, quality will take on greater importance.

## 13.3. Consequences

Higher-quality products may:

- Have greater commercial value.
- Last longer.
- Be more efficient.
- Satisfy higher-level needs.
- Be used in special constructions or products.

For example, a good-quality iron tool may last longer and improve agricultural productivity.

## 13.4. Production specialization

Settlements may develop a reputation for certain products.

A city may specialize in pottery, textiles, tools or agricultural products.

This may generate commercial and cultural advantages.

---

# 14. MAINTENANCE AND DETERIORATION

Buildings, tools and machines will require maintenance.

## 14.1. Buildings

Buildings may deteriorate due to:

- Time.
- Climate.
- Use.
- Lack of maintenance.
- Construction materials.

Deterioration may reduce their efficiency and eventually prevent them from operating.

## 14.2. Tools and machinery

Tools will suffer wear.

Their replacement or repair will require resources and labor.

Advanced technologies may improve their durability.

## 14.3. Automatic maintenance

The player will be able to set maintenance policies:

- Minimal.
- Normal.
- Preventive.
- Intensive.

Each option will have costs and consequences.

---

# 15. SEASONALITY AND ENVIRONMENTAL CONDITIONS

Production processes will depend on the environment.

## 15.1. Agriculture

Agricultural production will depend on:

- Seasons.
- Climate.
- Soil.
- Water.
- Crops.
- Technology.

## 15.2. Extractive activities

Some activities may be affected by:

- Extreme weather.
- Accessibility.
- Resource availability.
- Geographic conditions.
- Distance.

## 15.3. Artisanal and industrial activities

Transformation activities may operate throughout the year, although they will depend on inputs, energy and the condition of the buildings.

This will allow economies to have different productive rhythms depending on the season.

---

# 16. WORK ORGANIZATION AND PRODUCTION SCALE

The way production is organized will change over history.

## 16.1. Family production

In small societies, a family may produce different goods and satisfy a large part of its needs.

## 16.2. Artisanal production

With specialization, workers dedicated to a specific activity will appear.

Workshops may concentrate knowledge and tools.

## 16.3. Guilds and productive organizations

In certain societies, organizations may appear that regulate professions, quality, apprenticeship and access to certain activities.

## 16.4. Manufacturing

Production may be concentrated in establishments where different workers carry out specialized tasks.

## 16.5. Industry

In the final period of the game, larger-scale facilities may appear, with machinery, energy and a more advanced division of labor.

The evolution will not necessarily be the same across all civilizations.

---

# 17. OWNERSHIP AND CONTROL OF PRODUCTION

**Adopted decision: mixed model of private autonomy and player intervention.**

Buildings may belong to:

- The community.
- A family.
- A private owner.
- The State.
- Religious institutions.
- Commercial organizations.
- Companies.

Ownership will determine who controls the building and who receives its profits.

## 17.1. Communal production

Production and resources may be managed collectively.

## 17.2. Family production

Families may manage their own establishments and keep part of their surpluses.

## 17.3. Private production

Owners may hire workers, buy inputs and sell products.

They will have autonomy to make economic decisions within the conditions established by society.

## 17.4. State production

The State may control strategic facilities, set production targets and administer their profits.

## 17.5. Religious production

Religious institutions may own land, workshops and productive establishments.

## 17.6. Player intervention

The player will be able to influence private producers through:

- Taxes.
- Incentives.
- Subsidies.
- Regulations.
- Restrictions.
- Trade policies.
- Labor legislation.
- Ownership policies.

The goal is for the economy to have autonomy and not merely be a set of buildings that execute the player's direct orders.

---

# 18. INTERACTION WITH OTHER SYSTEMS

The production system will not operate in isolation.

## Economy

Determines costs, income, prices, surpluses and profitability.

## Population

Provides workers and consumers.

## Technology

Enables new methods and increases efficiency.

## Construction

Determines where buildings are located, how they grow and what infrastructure they need.

## Logistics

Allows raw materials, tools and products to be transported.

## Politics

Regulates taxes, wages, ownership and working conditions.

## Religion

May modify productive needs, constructions, materials and the activities of certain institutions.

## Culture and society

Influence professions, work organization and the aspirations of the population.

## Environment

Determines resource availability and production conditions.

---

# 19. PRODUCTION INTERFACE

The player must be able to understand how each building operates without having to constantly consult external information.

## 19.1. Building panel

Each building will display:

- Operating status.
- Current output.
- Maximum capacity.
- Assigned workers.
- Available positions.
- Available inputs.
- Stored products.
- Efficiency.
- Detected problems.
- Owner.
- Available improvements.
- Available expansions.
- Available historical evolution.

## 19.2. Production panel

It will allow all productive activities of a settlement to be observed.

Examples:

- Total food production.
- Material production.
- Raw material consumption.
- Production capacity.
- Employed workers.
- Inactive buildings.
- Productivity by sector.

## 19.3. Alerts

The system will notify important situations:

- Lack of workers.
- Lack of raw materials.
- Lack of tools.
- Transport problems.
- Interrupted production.
- Low productivity.
- Excess inventory.
- Deteriorated buildings.
- Available improvements.
- Newly enabled evolutions.

## 19.4. Contextual information

The player will be able to select any building and find out why it is producing less than expected.

Example:

**Bakery: reduced output.**

- Capacity: 100 loaves.
- Current output: 35 loaves.
- Workers: sufficient.
- Flour: insufficient.
- Problem detected: the mill is not receiving enough wheat.

The information must make it possible to quickly identify the source of problems.

---

# 20. CONFIGURABLE COMPLEXITY

The system will have different levels of depth.

## Basic level

- Buildings look for workers automatically.
- Production processes are automatic.
- Resources are distributed through simplified logistics.
- The player sets general priorities.
- Quality and working conditions have little direct intervention.

## Intermediate level

- The player controls priorities per building.
- Can configure production quantities.
- Can set reserves.
- Can manage wages and some working conditions.
- Can optimize production chains.
- Can control expansions.

## Advanced level

- Detailed management of workers.
- Specialization and training.
- Control of wages and contracts.
- Production planning.
- Inventory management.
- Detailed control of logistics.
- Cost and productivity optimization.
- Management of productive organizations.

Complexity must not alter the fundamental rules of the simulation. It will only change how much control the player has.

---

# 21. SIMULATION AND PERFORMANCE

The system must work both in small communities and in large cities.

## 21.1. Buildings

Each building will have an independent but optimized production simulation.

It will not be necessary to continuously process all of its calculations if there are no relevant changes.

## 21.2. Workers

In small settlements, individual workers can be simulated.

In large settlements, workers may be grouped statistically.

## 21.3. Production processes

Activities may be calculated by time intervals, instead of constantly updating each operation.

For example, a farm may update its output daily or seasonally.

## 21.4. Scalability

The architecture must allow new professions, buildings, recipes and production methods to be added without modifying the core systems.

---

# 22. MVP — FIRST PRODUCTION AND LABOR PROTOTYPE

The first prototype must demonstrate that buildings can produce autonomously and that workers are assigned correctly.

We will not attempt to implement all professions or technologies from the start.

## 22.1. Buildings

- Dwellings.
- Gatherer.
- Woodcutter.
- Quarry.
- Farm.
- Mill.
- Bakery.
- Storehouse.

## 22.2. Professions

- Gatherer.
- Woodcutter.
- Miner or quarryman.
- Farmer.
- Miller.
- Baker.
- Builder.

## 22.3. Resources

- Wood.
- Stone.
- Berries.
- Meat.
- Wheat.
- Flour.
- Bread.

## 22.4. Features

The prototype must allow:

1. Building productive structures.
2. Generating jobs.
3. Assigning workers automatically.
4. Producing resources.
5. Transforming raw materials.
6. Transporting products between buildings.
7. Storing inputs and products.
8. Consuming resources according to needs.
9. Setting production priorities.
10. Identifying production problems.
11. Observing the impact of a lack of workers.
12. Simulating productivity variations.

## 22.5. First test scenario

Create a small village of 30 inhabitants.

The player must build:

- A gathering area.
- A farm.
- A mill.
- A bakery.
- A storehouse.
- Dwellings.

The system must automatically organize the workers.

The goal will be to verify that:

- Inhabitants find work.
- Buildings receive inputs.
- Production chains function.
- Products reach consumers.
- The population can feed itself.
- A lack of resources generates problems.
- The player can intervene to solve them.

**Success criterion:** the village must be able to function for an extended period without the player having to manually assign each worker or personally transport each resource.

## 22.6. Visual evolution of the prototype

Although the MVP starts with few technologies, its architecture must provide for the possibility of evolving buildings from the outset.

It is not necessary to implement dozens of variants initially, but the system must be designed so that a structure can have:

- An initial version.
- Technological improvements.
- Modular expansions.
- Architectural transformations.
- New features.

Visual evolution must not be added as a decorative element at the end of development. It must be part of the design from the beginning.

---

# 23. CONFIRMED DESIGN DECISIONS

| System | Decision |
|---|---|
| Production organization | Based on buildings and processes |
| Production chains | Interdependent |
| Labor assignment | Automatic, with player intervention |
| Specialization | Primary profession and secondary skills |
| Productivity | Determined by multiple factors |
| Technology | Improves existing processes and enables new ones |
| Recipes | Variable and potentially alternative |
| Production control | The player sets priorities and the building adapts its output |
| Quality | Differentiated and gradually introduced |
| Maintenance | Necessary and evolving |
| Ownership | Mixed, with private autonomy and player intervention |
| Production scale | From family production to industry |
| Expansion | Modular, through modules and sectors |
| Building evolution | Technological, historical, religious, cultural, social, political and economic |
| Improvements | Visible notifications on the building and on the map |
| Architectural evolution | Visual and functional changes |
| Complexity | Configurable |
| Simulation | Individual in small settlements and aggregated in large cities |

---

# 24. OPEN DESIGN QUESTIONS

The following topics will need greater depth before their implementation:

1. Final productivity formulas.
2. Model of worker experience and learning.
3. Duration and conditions of profession changes.
4. Detailed model of wages and incentives.
5. Product quality system.
6. Rules for the autonomy of private owners.
7. Design of modules and expansions.
8. Exact conditions for enabling improvements.
9. Compatibility between old buildings and new technologies.
10. Costs and consequences of transformations.
11. Architectural evolution by civilization and region.
12. Balance between automatic production and player control.

These questions do not prevent progress on the general design. They can be resolved progressively.

---

# 25. FUNDAMENTAL PRINCIPLE OF THE SYSTEM

Production must represent a society's capacity to transform available resources into goods and services.

Labor must represent the organization of people, their knowledge and their capabilities to achieve it.

A civilization should not grow simply by building more buildings. It must develop better production methods, train workers, organize its resources, specialize its settlements and resolve the limitations of its economy.

But there is something even more important:

The evolution of a civilization must be visible.

The buildings the player constructed at the beginning will not be mere disposable elements. They may grow, transform, modernize and adapt to new historical conditions.

A small smithy may become a production complex. A family farm may evolve into an advanced agricultural holding. A small settlement may transform into a city where each neighborhood reflects a different stage of its history.

The world must preserve the traces of the player's decisions.

**The progress of a civilization will also be the progress of its capacity to produce, organize and transform the world around it.**

**And that progress must be something you can see, walk through and feel.**
