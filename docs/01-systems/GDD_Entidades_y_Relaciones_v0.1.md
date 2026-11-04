> **Document control:** 0.1 | Status: conceptual design; the decisions marked as confirmed are preserved from the source. **Scope:** this GDD is the detail source for its domain; the GDD Maestro establishes the shared vision. Cross-system conflicts are recorded in AUDIT_REPORT.md and are not resolved implicitly.

---

# GDD — World Entity and Relationship Model
**Version:** 0.1  
**Status:** Conceptual design

## 1. Objective

Define the fundamental entities that make up the game world, their attributes, identities, relationships and historical evolution.

This system constitutes a shared foundation for all other systems: population, economy, production, construction, technology, culture, religion, politics, diplomacy, exploration and war.

The goal is for the world not to be a collection of isolated statistics, but a coherent structure of entities that interact and evolve over time.

## 2. Fundamental principles

- All relevant entities have their own identity.
- Entities can relate to multiple entities simultaneously.
- Relationships can change, disappear and be re-established.
- Important relationships retain historical memory.
- Entities can evolve, transform, split, merge or disappear.
- Systems share the same entities and data; they do not create independent representations of the same element.
- The simulation can be simplified according to scale, importance and performance, without necessarily eliminating the conceptual identity.
- Complexity will be configurable.

## 3. Inhabitants and individuals

### 3.1 Population model

A hybrid model will be used:

- Identifiable individuals.
- Families and households.
- Demographic and social groups.
- Professions and labor sectors.
- Social classes and communities.

Individuals may be grouped to carry out aggregate simulations, especially in large populations.

Aggregation does not imply that inhabitants necessarily lose their identity. The level of detail simulated will depend on the importance, scale and configuration of the game.

### 3.2 Individual identity

Each individual will have a broad set of attributes:

- Identity and name.
- Age and sex.
- Family and kinship.
- Personal relationships.
- Place of birth and residence.
- Occupation and employers.
- Professions and skills.
- Health and physical condition.
- Education and knowledge.
- Culture and language.
- Religion and beliefs.
- Economic condition and property.
- Social class.
- Opinions and interests.
- Participation in organizations.
- Personal history and relevant events.

The attributes and their importance may vary according to era and historical context.

### 3.3 Prominent characters

Any individual may acquire relevance organically.

Examples:

- Political leaders.
- Merchants.
- Inventors.
- Scientists.
- Artists.
- Religious figures.
- Military figures.
- Explorers.
- Thinkers.

The player may identify, follow and consult specific characters, even when they are not important figures for the entire civilization.

Prominent characters may have more detailed biographies and relationships.

## 4. Families and households

Two related concepts will be distinguished:

**Family:** a unit of kinship and social relationships. It includes biological, marital, adoptive and other culturally recognized bonds.

**Household:** a unit of cohabitation and economic organization. It groups those who share a dwelling, resources or domestic activities.

Both structures may change over time.

A household can include several families, a family can be split across several households, and family structures can vary according to era and culture.

Families and households may have:

- Members.
- Internal relationships.
- A dwelling.
- Resources and property.
- Economic activities.
- Traditions.
- History and relevant events.

## 5. Buildings and structures

Each building will be an individual entity with its own identity.

Buildings will have:

- Identifier and identity.
- Location and geographic characteristics.
- Type and function.
- Owner and those responsible.
- Workers and occupants.
- Capacity and condition.
- Materials and construction techniques.
- Applied technology.
- Expansions and components.
- Economic or social activity.
- Maintenance and deterioration.
- Architectural evolution.
- History and relevant events.

Buildings will also be organized into shared categories and systems to facilitate their management.

A building may transform, expand, change function, become abandoned, turn into a ruin or be replaced, keeping its historical identity where appropriate.

The data structure must allow representing buildings with independent internal components, although the level of detail of those components will be implemented progressively.

## 6. Settlements

Villages, towns and cities will be entities with their own identity.

Their identity will arise from the interaction between inhabitants, buildings, economy, culture, geography and history.

Each settlement may have:

- Population and demographic composition.
- Families and communities.
- Buildings and infrastructure.
- Economy and specialization.
- Institutions.
- Culture and local identity.
- Religion and traditions.
- Administrative organization.
- Relationships with other settlements.
- Territory and influence.
- Events and historical memory.

Settlements may grow, change category, split, merge, be abandoned or disappear.

Their identity will not depend exclusively on their current population: they may retain traditions, buildings, names and legacies of former inhabitants.

## 7. Regions and territories

Regions will be represented as geographic, economic, cultural and administrative entities.

These dimensions will not necessarily coincide.

For example:
- A cultural region may span several political regions.
- A province may contain different cultural identities.
- An economic region may connect settlements from different territories.
- An administrative border may change without immediately altering the regional identity.

Regions will have their own characteristics, resources, population, settlements, institutions, relationships and historical evolution.

They will integrate with the Territorial Administration and Region Management system.

## 8. Organizations and institutions

Organizations will appear gradually when the necessary historical and social conditions exist.

They will not be limited to a rigid list of predetermined structures.

The following may exist:

- Extended families and clans.
- Guilds and artisan associations.
- Enterprises and trading organizations.
- Religious institutions.
- Governments and administrative bodies.
- Armies and military organizations.
- Schools, universities and academies.
- Political movements.
- Cultural organizations.
- Community associations.
- Diplomatic and supranational organizations.

Each organization may have:

- Identity and purpose.
- Members.
- Internal structure.
- Leadership.
- Properties and resources.
- Activities.
- Influence.
- External relationships.
- Rules and organization.
- History and events.

Organizations may arise, grow, transform, split, merge or disappear.

## 9. Relationships between entities

The world will use a system of multiple and dynamic relationships.

A single entity may maintain numerous simultaneous relationships with others.

Examples:

- An individual belongs to a family.
- Lives in a household.
- Works in a building.
- Is a member of a guild.
- Practices a religion.
- Has personal relationships with other inhabitants.
- Owns goods.
- Participates in a political organization.

Relationships will have, where appropriate:

- Type.
- Entities involved.
- Start date.
- Current status.
- Intensity or importance.
- Conditions.
- Origin.
- Associated events.
- End date.

Relationships may evolve and retain historical memory.

For example, a family may lose its property, a company may change owners, an individual may leave an organization or a city may cease to belong to a particular territory.

The system must allow consulting both current relationships and relevant historical relationships.

## 10. Identity and persistent history

Important entities will retain their identity over time.

Priority will be given to the history of:

- Relevant characters.
- Historical buildings.
- Settlements and cities.
- Organizations.
- Institutions.
- Regions.
- Civilizations.

The history may record:

- Foundation and origin.
- Transformations.
- Owners and those responsible.
- Changes of function.
- Relevant relationships.
- Events.
- Destructions and reconstructions.
- Cultural and architectural evolution.
- Legacies.

It is not required to permanently store every everyday action of every entity. Historical persistence must be selective and configurable, avoiding unnecessary data growth.

The goal is to be able to reconstruct the history of the important elements of the world.

## 11. Scalability and adaptive simulation

The simulation will be adaptive according to:

- The importance of the entity.
- The number of inhabitants.
- Economic activity.
- The complexity of relationships.
- Recent events.
- Relevance to the player.
- Simulation configuration.

Distance to the player will not be used as the sole criterion.

A distant city undergoing a major war may need more detail than a nearby region with no relevant events.

Entities may move between different levels of simulation, retaining the data necessary to maintain continuity and coherence.

The transition between detailed and aggregate simulation must not produce artificial demographic, economic or historical jumps.

## 12. Integration with other systems

The model will be shared by all systems.

Examples:

- **Population:** individuals, families, households, groups and social classes.
- **Economy:** owners, producers, workers, enterprises and goods.
- **Production:** buildings, workers, recipes and productive organizations.
- **Construction:** buildings, materials, owners, architects and construction works.
- **Technology:** specialists, institutions, knowledge and discoveries.
- **Religion:** individuals, communities, institutions and religious organizations.
- **Culture:** individuals, groups, settlements and traditions.
- **Politics:** governments, leaders, institutions and factions.
- **Diplomacy:** civilizations, leaders, diplomats and treaties.
- **War:** armies, commanders, settlements and territories.
- **Exploration:** expeditions, individuals, organizations and regions.

The systems must consult and modify the same entities, respecting the rules of each domain.

## 13. Property and ownership

Concepts that must not be confused will be distinguished:

- Ownership of natural resources.
- Exploitation rights.
- Ownership of buildings and facilities.
- Ownership of goods and merchandise.
- Operational control of activities.
- State administration and regulation.

This makes it possible to maintain the decision that natural resources belong to the State or civilization, while private producers, families and organizations can administer activities, operate facilities and manage goods in accordance with the established rights.

Ownership and control may evolve historically.

## 14. Interface and consultation

The player will be able to consult entities and their relationships from the 3D world and from specific tools.

The following are contemplated:

- Individual profiles.
- Family trees.
- Organizational charts.
- Relationships between organizations.
- Histories of buildings and cities.
- Chronologies.
- Territorial maps.
- Economic and social networks.
- Character biographies.

The depth of information will depend on the chosen complexity.

## 15. Configurable complexity

The system must allow adjusting:

- The level of individual detail.
- Demographic aggregation.
- The depth of relationships.
- Historical persistence.
- Organization simulation.
- Character tracking.
- Update frequency.
- The detail of distant entities.

The detail configuration must not arbitrarily change the fundamental rules of the world.

## 16. MVP

The first prototype must contemplate:

- Identifiable individuals.
- Basic families and households.
- Individual buildings.
- Settlements.
- Relationships between inhabitants, households and buildings.
- Simple organizations.
- Basic property.
- An elementary history of entities.
- The ability to group population.
- Persistent identifiers.

The MVP does not need to implement all social relationships, historical organizations or complete biographies. It must establish an extensible structure that allows incorporating them without rebuilding the core model.

## 17. Acceptance criteria

The system will be conceptually satisfactory when:

1. An inhabitant can have their own identity, family, work, dwelling and relationships.
2. Families and households can change over time.
3. Buildings retain identity and history.
4. Cities develop their own identity.
5. Regions can have different cultural, economic, geographic and political dimensions.
6. Organizations can arise and evolve organically.
7. Relationships can change without losing their relevant history.
8. Important entities can be consulted and followed.
9. The simulation can scale without breaking the continuity of the world.
10. All systems share coherent entities and data.

## 18. Guiding principle

**The world will not be made up of isolated objects, but of entities with identity, relationships and memory. Each inhabitant, building, city and organization will be part of a shared history that evolves over the centuries.**
