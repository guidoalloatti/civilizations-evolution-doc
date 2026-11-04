> **Document control:** 0.2 | Status: conceptual design; the decisions marked as confirmed are preserved from the source. **Scope:** this GDD is the detail source for its domain; the GDD Maestro establishes the shared vision. Cross-system conflicts are recorded in AUDIT_REPORT.md and are not resolved implicitly.

---

# GDD — ECONOMIC SYSTEM
**Version:** 0.2  
**Status:** Conceptual design  
**Historical period:** Paleolithic – approximately 1800 AD

---

# 1. Overview

The economic system will be one of the game's fundamental pillars.

The goal is to simulate the economic evolution of a civilization from its most primitive forms of subsistence to complex economies, with labor specialization, markets, private property, trade, financial institutions and industries.

The economy must not function as a simple system of resource gathering and consumption. It must represent a network of relationships between:

- Natural resources.
- Production.
- Population.
- Labor.
- Consumption.
- Storage.
- Transport.
- Trade.
- Property.
- Finance.
- Institutions.
- Technology.
- Environment.

Economic decisions will have social, political and cultural consequences.

## 1.1. Design principles

1. **Historical coherence:** economic possibilities evolve with technology and society.
2. **Freedom:** the player can develop their civilization along different paths.
3. **Specialization:** not every settlement must produce everything.
4. **Interdependence:** resources and products must form economic chains.
5. **Real consequences:** scarcity, mismanagement and economic decisions must have effects.
6. **Configurable complexity:** the player can choose how much detail to manage.
7. **Organic evolution:** economic institutions appear when the necessary social and technological conditions exist.

---

# 2. General model of the economy

The economy is organized into six broad systems:

1. Resources and raw materials.
2. Production and transformation.
3. Population and labor.
4. Consumption and needs.
5. Logistics, storage and transport.
6. Markets, trade and finance.

All these systems will be connected.

For example:

The population needs to feed itself. To produce food, workers, land, tools and raw materials are needed. Food must be transported and stored. Surpluses can be traded. Trade generates wealth and can encourage the specialization of settlements.

---

# 3. Resources

Resources represent all the elements that a civilization can obtain, produce, transform or consume.

## 3.1. Resource categories

### Natural resources
- Wood.
- Stone.
- Clay.
- Sand.
- Iron.
- Copper.
- Tin.
- Gold.
- Silver.
- Coal.
- Water.
- Hides.
- Plant fibers.

### Food
- Berries.
- Fruit.
- Meat.
- Fish.
- Cereals.
- Vegetables.
- Milk.
- Eggs.
- Honey.

### Processed products
- Flour.
- Bread.
- Cheese.
- Preserved meat.
- Oil.
- Wine.
- Beer.
- Textiles.
- Leather.
- Pottery.

### Manufactured products
- Tools.
- Weapons.
- Armor.
- Furniture.
- Prefabricated structures.
- Machinery.
- Vessels.

### Services
- Transport.
- Education.
- Health.
- Administration.
- Religious services.
- Commercial services.

Services may be incorporated progressively as the civilization develops.

## 3.2. Resource properties

Each resource may have different characteristics:

- Available quantity.
- Geographic location.
- Quality.
- Weight and volume.
- Economic value.
- Production cost.
- Production time.
- Shelf life.
- Technological requirements.
- Storage possibilities.
- Possible uses.

Not all resources will behave the same way.

For example, perishable foods will need preservation conditions, while minerals may be stored for long periods.

## 3.3. Renewable and non-renewable resources

Natural resources will behave differently:

**Renewable:**
- Forests.
- Fish.
- Animals.
- Crops.
- Water, depending on local availability.

**Non-renewable:**
- Minerals.
- Metals.
- Coal.
- Other geological deposits.

Overexploitation may cause scarcity, environmental degradation or depletion of deposits.

---

# 4. Production and transformation

Production will be one of the game's central systems.

Productive buildings will transform resources through processes that require time, workers and raw materials.

## 4.1. Production recipes

Each production process will have:

- Input resources.
- Output products.
- Production time.
- Number of workers needed.
- Required technology.
- Necessary tools.
- Maximum capacity.
- Energy consumption, where applicable.
- Maintenance costs.

Example:

**Bakery**

Inputs:
- Flour.
- Water.
- Fuel.

Output:
- Bread.

Requirements:
- A suitable building.
- Workers.
- The corresponding technology.

## 4.2. Productivity

Productivity will depend on different factors:

- Number of workers.
- Training.
- Available tools.
- Technology.
- Quality of raw materials.
- Environmental conditions.
- Distance to resources.
- Work organization.

Technology will make it possible to improve productivity without necessarily increasing the number of workers.

## 4.3. Productive capacity

Each building will have a maximum capacity.

The player will be able to build more buildings, improve existing ones or incorporate new technologies to increase production.

Not all buildings will need to operate permanently. Some may work seasonally or according to resource availability.

---

# 5. Population and labor

The population will be the engine of the economy.

Inhabitants will not simply be units that consume food: they will be workers, producers, consumers and participants in society.

## 5.1. Hybrid population model

A hybrid model combining individuals and social groups will be used.

### Small settlements

Inhabitants will be simulated individually.

Each individual may have:
- Age.
- Family.
- Dwelling.
- Profession.
- Skills.
- Needs.
- Economic condition.
- Social relationships.

This will allow observing how a small community functions and how it organizes itself.

### Large settlements

As the population grows, individuals may automatically group into structures such as:

- Families.
- Professional groups.
- Economic sectors.
- Social classes.
- Cultural groups.

The groups will retain their own statistical characteristics and behaviors.

### Representation of interests

Groups may have representatives who express their interests and demands.

For example:
- Peasants demanding better conditions.
- Merchants requesting greater commercial freedom.
- Artisans seeking protection for their activities.
- Landowners defending their properties.
- Workers demanding better wages.

This will connect the economy with the political and social systems.

## 5.2. Worker assignment

**Decision adopted: automatic assignment with player intervention.**

Inhabitants will seek job opportunities according to:

- Availability of positions.
- The community's needs.
- Wages or benefits.
- Skills.
- Distance to the workplace.
- Working conditions.
- Economic priorities.

The player will be able to intervene by setting:

- Production priorities.
- Worker limits per building.
- Priority sectors.
- Labor restrictions.
- Employment policies.

The goal is to avoid the player having to manually assign each inhabitant to a building.

## 5.3. Labor specialization

As the civilization develops, increasingly specialized professions will appear.

Examples:
- Gatherers.
- Hunters.
- Farmers.
- Builders.
- Miners.
- Blacksmiths.
- Carpenters.
- Merchants.
- Administrators.
- Engineers.

Specialization will improve productivity, but it will also generate dependence between different economic sectors.

## 5.4. Wages and working conditions

In advanced stages, different remuneration models may appear:

- Community labor.
- Family labor.
- Forced labor.
- Wage labor.
- Slave labor, where historically appropriate.
- Specialized labor.

Wages and working conditions may affect satisfaction, productivity, migration and social conflicts.

---

# 6. Consumption and needs

The population will have needs that will evolve with the development of the civilization.

## 6.1. Basic needs

- Food.
- Water.
- Housing.
- Protection.
- Clothing.

Failure to meet these needs may cause disease, mortality, emigration and loss of productivity.

## 6.2. Advanced needs

As society evolves, new demands will appear:

- Better food.
- Higher-quality clothing.
- Education.
- Health.
- Entertainment.
- Religious services.
- Luxury goods.
- Culture.
- Security.

Not all social sectors will have the same needs or the same access to products.

## 6.3. Social differences

The economic level of each group will influence its consumption.

A peasant family will not have the same possibilities as a wealthy merchant or a member of the nobility.

These differences may generate:
- Inequality.
- Social aspirations.
- Conflicts.
- Social mobility.
- Cultural and political changes.

---

# 7. Agriculture and food production

Agriculture will have a specific system due to its historical importance.

## 7.1. Agricultural cycles

Crops will depend on:
- Seasons.
- Climate.
- Soil type.
- Water availability.
- Technology.
- Seeds.
- Labor.

Not all crops can be produced in all regions.

## 7.2. Seasonality

Agricultural production will vary throughout the year.

The player must plan:
- Sowing.
- Harvest.
- Storage.
- Consumption.
- Reserves for periods of scarcity.

## 7.3. Food crises

Poor harvests may generate:
- Scarcity.
- Rising prices.
- Hunger.
- Migrations.
- Social conflicts.
- Political instability.

Storage and planning will be essential to avoid these situations.

---

# 8. Storage and logistics

Resources must be stored and distributed.

## 8.1. Storage

There will be different types of storage:

- General storehouses.
- Granaries.
- Silos.
- Commercial warehouses.
- Specialized storehouses.
- Port warehouses.

Each will have different capacities and conditions.

Perishable products may deteriorate over time if not stored correctly.

## 8.2. Physical transport

**Decision adopted: resources will be transported physically from the start.**

Products must move between the places where they are produced, stored and consumed.

For example:

A farm produces wheat. The wheat must be moved to a mill. The mill produces flour. The flour must reach a bakery. Finally, the bread must be distributed among the population.

This will make the location of buildings and the organization of settlements economically important.

## 8.3. Logistics evolution

Logistics will evolve historically.

At first:
- Manual transport.
- Pack animals.
- Simple carts.

Later:
- Roads.
- Specialized carts.
- Commercial ships.
- Port infrastructure.
- More efficient transport systems.

## 8.4. Configurable complexity

Transport will be physical, but it will not necessarily require controlling every movement.

The simulation may be simplified internally to avoid excessive micromanagement.

The player will be able to choose between automatic management and more detailed logistics management.

---

# 9. Markets and prices

Markets will connect production with consumption and trade.

## 9.1. Price formation

Prices may depend on:

- Supply.
- Demand.
- Local availability.
- Production costs.
- Transport costs.
- Distance.
- Quality.
- Seasonality.
- Taxes.

Prices will not necessarily be the same in all settlements.

## 9.2. Local and regional markets

Markets may evolve from direct exchange to complex commercial networks.

A region with an abundance of wood may sell it to another region that has a shortage.

This will encourage geographic specialization.

## 9.3. Surpluses and scarcity

The accumulation of surpluses may lower prices and encourage trade.

Scarcity may generate price increases, speculation and social tensions.

In advanced stages, expectations about future harvests, resource availability and prices may appear.

---

# 10. Trade

Trade will make it possible to exchange resources between buildings, settlements and civilizations.

## 10.1. Three levels of complexity

### Basic level: automatic

The player sets which resources they need and which they have available to exchange.

The system carries out the operations automatically.

Ideal for players who prefer to concentrate on construction and development.

### Intermediate level: commercial management

The player will be able to configure:
- Maximum and minimum quantities.
- Reserves.
- Contracts.
- Budgets.
- Commercial priorities.

### Advanced level: logistics management

The player will be able to manage:
- Trade routes.
- Vehicles.
- Ships.
- Cargo.
- Transport capacity.
- Costs.
- Risks.
- Tariffs.
- Commercial relationships.

## 10.2. Trade between settlements

Settlements may specialize and depend on each other economically.

For example:
- An agricultural region produces food.
- A mining region produces metals.
- A city concentrates manufacturing.
- A port connects the different regions.

## 10.3. International trade

In advanced stages, trade between civilizations will be incorporated.

This will allow:
- Imports.
- Exports.
- Trade treaties.
- Tariffs.
- Monopolies.
- Trade blockades.
- Economic competition.

---

# 11. Money and finance

Money will not necessarily be present from the start.

Its appearance will depend on historical, social and economic development.

## 11.1. Monetary evolution

The economy may go through different stages:

1. Direct exchange.
2. Exchange based on valuable goods.
3. Goods used as means of payment.
4. Coins.
5. More complex monetary systems.
6. Credit and banking.
7. Modern finance.

The evolution will not be strictly linear or identical for all civilizations.

## 11.2. Public treasury

The player will manage the economic resources of the institutions they govern.

They will be able to manage:
- Income.
- Expenditure.
- Reserves.
- Taxes.
- Investments.
- Debts.
- Budgets.

## 11.3. Private finance

In advanced stages the following will appear:
- Wealthy merchants.
- Moneylenders.
- Banks.
- Investors.
- Trading companies.
- Enterprises.

These actors may have their own interests and participate in the civilization's economy.

---

# 12. Economic property

**Decision adopted: a mixed and evolving model.**

Property will not belong exclusively to the player.

Its organization will depend on the era, the institutions and the characteristics of each society.

## 12.1. Types of property

- Community.
- Family.
- Private.
- State.
- Religious.
- Corporate.
- Feudal or manorial.

## 12.2. Evolution of property

In primitive societies, community and family forms will predominate.

With economic development, private property, religious institutions owning land, large landowners, enterprises and state organizations may appear.

## 12.3. Consequences

Property will determine:
- Who controls production.
- Who receives the benefits.
- Who pays taxes.
- Who makes decisions.
- Which groups concentrate wealth.
- Which sectors have political influence.

The player will be able to influence these models through institutions, laws and policies.

---

# 13. Taxes and economic policies

Political institutions will be able to intervene in the economy.

## 13.1. Taxes

Depending on the era and the political system, the following may exist:

- Production taxes.
- Land taxes.
- Commercial taxes.
- Tariffs.
- Tributes.
- Income taxes.
- Service fees.

## 13.2. Economic policies

The player will be able to implement policies such as:

- Subsidies.
- Commercial protection.
- Public investment.
- Price controls.
- Market regulation.
- Incentives for certain sectors.
- Trade restrictions.
- Investment in infrastructure.

Each policy will have benefits and costs.

A policy favorable to merchants could harm certain local producers, while a protectionist policy could favor domestic industries but make products more expensive.

---

# 14. Historical economic evolution

The economy will evolve according to the technological, social and cultural possibilities of each civilization.

## Paleolithic

- Gathering.
- Hunting.
- Fishing.
- Community production.
- Direct exchange.
- Low specialization.

## Neolithic

- Agriculture.
- Livestock farming.
- Sedentism.
- Storage.
- Surpluses.
- Initial specialization.
- Family and community property.

## Bronze and Iron Ages

- Metallurgy.
- Specialized trades.
- Regional trade.
- Markets.
- Greater division of labor.
- Growth of settlements.

## Antiquity

- Large commercial networks.
- Coins.
- Taxes.
- Maritime trade.
- Large agricultural estates.
- Specialized craftsmanship.
- Economic institutions.

## Middle Ages

- Feudalism in certain societies.
- Guilds.
- Trade fairs.
- Regional markets.
- Religious property.
- Development of trade routes.
- Greater urban specialization.

## Early Modern period

- International trade.
- Maritime expansion.
- Banking.
- Credit.
- Trading companies.
- Manufacturing.
- More complex tax systems.

## Pre-industrial period

- Greater division of labor.
- Large-scale manufacturing.
- Investments.
- Advanced financial systems.
- Global trade.
- Initial mechanization.

These stages are historical references, not mandatory paths. Civilizations may evolve in different ways.

---

# 15. Environment and sustainability

The economy will have consequences for the natural environment.

Resource exploitation may cause:

- Deforestation.
- Erosion.
- Depletion of mineral deposits.
- Overfishing.
- Loss of soil fertility.
- Pollution.
- Water scarcity.

The player must find a balance between economic growth and resource availability.

Environmental degradation may affect productivity and the living conditions of the population.

---

# 16. Economic interface

The interface must make it possible to quickly understand how the economy works.

## General panel

- Population.
- Total production.
- Consumption.
- Available resources.
- Income.
- Expenditure.
- Economic balance.

## Resource panel

- Available quantity.
- Production.
- Consumption.
- Stocks.
- Reserves.
- Trends.

## Building panel

- Current production.
- Capacity.
- Workers.
- Required resources.
- Efficiency.
- Operational status.

## Population panel

- Total population.
- Labor distribution.
- Needs.
- Living conditions.
- Social groups.
- Unemployment.

## Commercial panel

- Purchases.
- Sales.
- Prices.
- Routes.
- Contracts.
- Imports.
- Exports.

The interface must include alerts when important problems exist, such as food scarcity, a lack of workers or logistics disruptions.

---

# 17. Simulation and performance

The system must be able to handle everything from small villages to large cities.

To achieve this, a hybrid simulation will be used.

## 17.1. Detailed simulation

In small settlements, individuals, workers, movements and needs will be simulated in greater detail.

## 17.2. Aggregate simulation

In large cities, individuals and activities will be grouped into statistical units.

Groups of workers, families and economic sectors may be simulated.

## 17.3. Consistency

The transition between individual and aggregate simulation must not arbitrarily change the economic rules.

The goal is to maintain the coherence of the system regardless of the size of the civilization.

---

# 18. Initial economic prototype (MVP)

The first implementation must be small and controllable.

We will not attempt to build the entire historical economy from the start.

## 18.1. Initial resources

- Wood.
- Stone.
- Berries.
- Meat.
- Wheat.
- Flour.
- Bread.

## 18.2. Initial buildings

- Dwelling.
- Gatherer.
- Woodcutter.
- Quarry.
- Farm.
- Mill.
- Bakery.
- Storehouse.

## 18.3. Functionalities

The prototype must allow:

1. Creating a settlement.
2. Building buildings.
3. Assigning workers automatically.
4. Gathering resources.
5. Producing food.
6. Transforming raw materials.
7. Transporting resources physically.
8. Storing products.
9. Distributing food.
10. Consuming resources according to the population.
11. Experimenting with seasonality.
12. Observing production and supply problems.

## 18.4. MVP objective

Demonstrate that a basic economy can function without constant player intervention.

The player must be able to concentrate on designing and developing their settlement while the economy responds to their decisions.

---

# 19. Confirmed design decisions

The following decisions are established for this version:

| System | Decision |
|---|---|
| Property | Mixed and evolving model |
| Labor assignment | Automatic, with player intervention |
| Transport | Physical from the start, with simplified simulation |
| Population | Hybrid model: individuals and groups |
| Economic complexity | Configurable according to play style |
| Trade | Three management levels: basic, intermediate and advanced |
| Historical evolution | Flexible, non-linear and conditioned by society and technology |
| Simulation | Detailed in small settlements and aggregate in large cities |

---

# 20. Pending design questions

These topics still need more detailed definitions:

1. How productivity is calculated exactly.
2. How wages and labor incentives are determined.
3. How markets and price formation work.
4. How resources are distributed automatically.
5. How the ownership of buildings and land is represented.
6. How transport and logistics are managed.
7. How the economies of different settlements are integrated.
8. How financial institutions appear and evolve.
9. Which resources exist in each historical period.
10. How the economy is balanced to avoid dominant strategies.

---

# 21. Fundamental principle of the economic system

The economy must not be an isolated system of gathering, production and consumption.

It must be the mechanism that connects natural resources with social organization, technological development, political institutions and the historical evolution of the civilization.

A civilization must develop because it manages to better organize its resources, its population and its knowledge, not simply because it accumulates points or unlocks buildings.

**The goal is for each civilization to be able to develop its own economy, coherent with its environment, its decisions and its historical evolution.**
