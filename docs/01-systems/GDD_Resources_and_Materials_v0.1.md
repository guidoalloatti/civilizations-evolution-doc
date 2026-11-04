> **Document control:** 0.1 | Status: conceptual design; decisions marked as confirmed are preserved from the source. **Scope:** this GDD is the detail source for its domain; the Master GDD establishes the common vision. Cross-system conflicts are recorded in AUDIT_REPORT.md and are not resolved implicitly.

---

# GDD — Global Resources and Materials System

**Version:** 0.1  
**Status:** Conceptual design

## 1. Overview

The game will use a global, unified resource system.

Each resource will have a unique identity and may be used by all the systems that need it: economy, production, construction, population, trade, transport, technology, institutions and military activities.

There will be no duplicate resources to represent the same material across different systems.

For example, the wood produced by a woodcutter will be the same wood that a construction can use, an industry can consume, a warehouse can store or a trader can transport.

The catalog, detail and complexity will be configurable.

## 2. Resource classification

Resources will have two complementary classifications.

### 2.1. By nature and category

- Natural resources.
- Agricultural resources.
- Livestock resources.
- Fishing resources.
- Food and drink.
- Construction materials.
- Metals and minerals.
- Fuels and energy sources.
- Textile raw materials.
- Manufactured products.
- Tools and equipment.
- Consumer goods.
- Special trade products.
- Strategic resources.

### 2.2. By economic function

- **Raw materials:** resources extracted or produced directly from nature.
- **Intermediate products:** transformed materials that serve as inputs.
- **Final products:** goods intended for consumption, construction, trade or direct use.

The same resource may fulfill different functions depending on its use.

## 3. Level of detail

The level of detail will be configurable.

The intermediate configuration will be the initial reference.

Examples:
- Wood.
- Stone.
- Clay.
- Sand.
- Iron.
- Copper.
- Tin.
- Wheat.
- Flour.
- Bread.
- Meat.
- Hides.
- Wool.
- Leather.

In more advanced configurations there may be additional varieties and properties.

The catalog may be expanded with technological, commercial and cultural progress, without needing to replace the basic resources.

## 4. Quality and properties

Resources may acquire differentiated quality and properties.

Quality will appear progressively according to historical and technological development.

It may depend on:
- Natural characteristics.
- Origin.
- Extraction techniques.
- Production methods.
- Tools used.
- Labor specialization.
- Processing.
- Preservation.
- Technology.

In early stages, the differences may be simplified. As the civilization develops, more precise distinctions may be incorporated.

Quality may influence:
- Price.
- Durability.
- Performance.
- Production efficiency.
- Commercial value.
- Prestige.
- Consumption preferences.

Not all resources will need to have quality in the same way.

## 5. Natural resources

Natural resources will behave differently according to their nature.

The following will be considered:
- Renewable resources.
- Partially renewable resources.
- Non-renewable resources.
- Exhaustible resources.
- Resources regenerable under certain conditions.

Exploitation may cause:
- Deforestation.
- Soil depletion.
- Overfishing.
- Reduction of fauna.
- Depletion of deposits.
- Environmental degradation.

Renewable resources may become temporarily depleted or degraded if their exploitation exceeds their capacity for regeneration.

Recovery will depend on environmental characteristics and on exploitation practices.

## 6. Storage

Storage will evolve historically.

There may be:
- General warehouses.
- Granaries.
- Silos.
- Specialized warehouses.
- Cellars.
- Commercial warehouses.
- Port warehouses.
- Special facilities.

Capacities and conditions will depend on infrastructure and technology.

Stored resources may be subject to:
- Maximum capacity.
- Deterioration.
- Pests.
- Humidity.
- Theft.
- Losses.
- Maintenance costs.

The level of detail will be configurable.

## 7. Deterioration and preservation

Resources may deteriorate according to their properties.

Possible factors include:
- Nature of the product.
- Storage time.
- Temperature.
- Humidity.
- Facilities.
- Preservation techniques.
- Packaging.
- Transport.

Food, drink, organic materials and certain chemical or manufactured products may have specific behaviors.

The complexity and frequency of deterioration will be configurable.

## 8. Consumption and use

Resources will be used through two integrated mechanisms:

1. Consumption associated with the needs of population, households, institutions and organizations.
2. Consumption associated with production activities, construction, maintenance, transport and other operations.

Systems must query the common inventory.

Consumption will depend on:
- Needs.
- Economic capacity.
- Availability.
- Priorities.
- Technology.
- Quality.
- Local conditions.

Unmet needs may generate social and economic consequences according to their importance and duration.

## 9. Geographic distribution

Resources will have differentiated geographic distributions.

Availability will depend on:
- Geology.
- Biomes.
- Soils.
- Climate.
- Hydrology.
- Flora and fauna.
- Marine resources.
- Environmental conditions.

The world configuration will allow abundance and distribution to be adjusted.

The location of resources will generate regional advantages and limitations, influencing settlements, production specialization and trade.

## 10. Resource substitution

Production processes and constructions may use substitutes when possible.

The alternatives may depend on:
- Technology.
- Availability.
- Price.
- Distance.
- Quality.
- Physical properties.
- Building or production traditions.

Examples:
- Different materials for construction.
- Alternative fuels.
- Tools made from different materials.
- Alternative agricultural inputs.

Substitution will not be universal: it must respect the physical and technical limitations of each activity.

## 11. Property and control

Resources will belong to the State or to the civilization.

This does not mean that all production activities must be administered directly by the government.

Producers will be able to operate autonomously and manage their activities through economic mechanisms.

The system must allow:
- Resource allocation.
- Public inventories.
- Budgets.
- Purchases and sales.
- Taxes.
- Subsidies.
- Incentives.
- Production priorities.
- Regulations.
- Strategic reserves.

Private ownership of buildings and enterprises may exist as an organizational form, but it will not alter the general rule of resource title.

## 12. Special and strategic resources

There will be resources with commercial, cultural, economic or strategic importance.

Examples:
- Salt.
- Spices.
- Silk.
- Dyes.
- Precious stones.
- Horses.
- Special metals.
- Medicinal products.
- Luxury craft goods.

These resources may generate:
- Trade routes.
- Regional specialization.
- Prestige.
- Revenue.
- Economic dependencies.
- Diplomatic interests.
- Conflicts over access or control.

Not all strategic resources will have military importance: some will be relevant mainly for their economic or cultural value.

## 13. Money and monetary unit

The game will use a universal monetary unit from the start.

It will not be mandatory to simulate the historical transition from barter to monetary systems.

The monetary unit will allow:
- Valuing resources.
- Setting prices.
- Paying wages.
- Calculating taxes.
- Managing budgets.
- Financing constructions.
- Carrying out trade.
- Measuring wealth and economic activity.

Prices may evolve according to supply, demand, scarcity, distance, transport, quality and commercial conditions.

The existence of a common monetary unit will be an abstraction of the economic model, not a historical claim that all societies used money in the same way.

## 14. Units and accounting

Resources will use abstract units as their main representation.

This will allow consistency and performance to be maintained without forcing every operation to be converted into kilograms, liters or tons.

Units may represent approximate physical quantities when useful for the interface.

The unit scale must be consistent across:
- Production.
- Consumption.
- Construction.
- Storage.
- Transport.
- Trade.

Precision will be configurable.

## 15. Integration with other systems

The global resource system will be shared by:

- Economy.
- Production and labor.
- Construction and architecture.
- Population and society.
- Transport and logistics.
- Trade.
- Technology.
- Agriculture and livestock.
- Geography and environment.
- Territorial administration.
- Diplomacy.
- War.

All systems will use the same catalog and the same general rules.

## 16. MVP

First implementation:

- Basic resource catalog.
- Classification by category and function.
- Abstract units.
- Shared inventories.
- Production and consumption.
- Storage.
- Renewable and exhaustible natural resources.
- Basic prices.
- Substitutes defined for some recipes.
- Simplified food deterioration.

Initial resources:
- Wood.
- Stone.
- Clay.
- Berries.
- Meat.
- Wheat.
- Flour.
- Bread.

The system must allow the same quantity of resources to be used and accounted for coherently throughout the simulation.

## 17. Guiding principle

**Each resource represents something real within the simulated world. There must not be a different version of the same material for each system: the economy, production, construction and trade share a single material reality.**
