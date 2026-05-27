# Municipal Configuration

Customized for the City of Elgin, Illinois. This configuration is used by all skills and commands in the municipal-governance plugin.

## Municipality

- **Name**: City of Elgin
- **State**: Illinois
- **Population**: ~115,000
- **Government Type**: Council-Manager (adopted 1954)
- **Home Rule**: Yes (Illinois home rule municipality; population exceeds 25,000 threshold per Article VII, Section 6 of the Illinois Constitution)

## Council Structure

- **Governing Body Title**: City Council
- **Members**: 8 Council Members + Mayor (9 total)
- **Terms**: Four-year terms, staggered
- **Districts**: At-Large (all members serve the entire community)
- **Meeting Schedule**: 2nd and 4th Wednesday, 6:00 PM
- **Meeting Location**: Council Chambers, 2nd Floor, City Hall, 150 Dexter Court, Elgin, IL 60120
- **Committee of the Whole**: Precedes regular council meetings; less formal discussion format for items before they move to a final vote

### Current Council Members (as of March 2026)

| Member | Role |
|--------|------|
| David Kaptain | Mayor |
| Diana Alfaro | Council Member |
| Corey Dixon | Council Member |
| Dustin Good | Council Member |
| Rosamaria Martinez | Council Member |
| Anthony Ortiz | Council Member |
| Tish S. Powell | Council Member |
| F. John Steffen | Council Member |
| Steve Thoren | Council Member |

## Key Code References

- **Municipal Code Provider**: Municode
- **Code URL**: https://library.municode.com/il/elgin
- **Zoning Code**: Title 19
- **Subdivision Code**: Title 18 - Subdivisions
- **Building Code**: Title 16 - Buildings and Construction (Chapter 16.04 — Building Code; also includes Existing Building Code (16.06), Property Maintenance (16.12), Plumbing (16.20), Electrical (16.24), Fire Prevention (16.28), HVAC (16.32), Residential Dwelling (16.36), Energy Conservation (16.38))

## Agenda Management

- **System**: CivicPlus CivicEngage (city website platform with integrated agenda center)
- **Agenda Center URL**: https://elginil.gov/agendacenter
- **Agendas & Minutes Archive**: https://elginil.gov/archive.aspx
- **Public URL**: https://elginil.gov
- **Note**: Plugin works with file-based agenda packets (uploaded PDFs) when no structured agenda API is available. Council meeting videos available at elginvideo.cityofelgin.org and Channel 17/99.

### Active TIF Districts

| District Name | Creation Date | Expiration Date | Purpose |
|--------------|---------------|-----------------|---------|
| Central Area TIF | [Verify creation date] | [Verify expiration — HB 3662 may extend] | Downtown revitalization |
| Bluff City TIF | [Verify creation date] | [Verify expiration — HB 3662 may extend] | Economic development |
| U.S. Route 20 TIF | [Verify creation date] | [Verify expiration] | Corridor development |

### Special Districts/Authorities

- Elgin spans both **Kane County** and **Cook County** (Hanover Township portion in Cook County)
- Multiple overlapping taxing districts depending on property location

## Budget Context

- **Fiscal Year**: Calendar Year (January–December)
- **General Fund Size**: ~$175–200M annually
- **Major Revenue Sources**: Property tax, sales tax, utility tax, state income tax sharing, home rule sales tax
- **Tax Limitations**: Illinois PTELL (Property Tax Extension Limitation Law) applies to non-home-rule levies; home rule authority provides additional flexibility
- **Bond Rating**: AAA (Fitch) / AA+ (S&P) — per city financial rating information page; 2025 GO Corporate Purpose Bonds issued
- **Note**: General fund property tax levy held flat for 11+ consecutive years (as of 2025); combined levy increases driven by required pension contributions

### Fiscal Impact Thresholds

These thresholds are used by commands to flag items for attention:

- **Critical Fiscal Impact**: $500,000 — triggers 🔴 classification
- **Significant Fiscal Impact**: $100,000 — triggers 🟡 classification
- **Contract Authority Limit**: $50,000 (requiring council approval)
- **Budget Amendment Threshold**: $25,000 (requiring council approval for line-item transfers)

## Policy Priorities

Current administration/council priorities (as of March 2026):

1. **Welcoming City / Immigration Response** — Council is debating whether to codify existing welcoming policies via ordinance (including municipal ID program and legal defense fund) or pursue a policy-based approach to avoid federal attention. Context: Elgin experienced major ICE enforcement operations in September 2025 (Operation Midway Blitz), including a raid led by DHS Secretary Noem that detained 7 people (2 were U.S. citizens). Council approved resolutions urging legislators to ban ICE agents from wearing masks and creating ICE-free zones on city property. Council is split on vehicle (ordinance vs. policy), united on values of protecting vulnerable residents. Some members seeking to add procurement policies to the welcoming city ordinance.
2. **Single-Use Plastic Bag Ban** — Ordinance approved February 2026, effective June 2027. Affects 42 large retailers; exempts small businesses, restaurants, gas stations. Paper bags available for $0.10; SNAP recipients exempt. Community survey showed 57% opposition, 38% support. Implementation and compliance monitoring ongoing.
3. **USDR Partnership / AI & Digital Government** — Partnership with United States Digital Response to explore AI applications in municipal government. Currently limited council engagement (one member actively involved). Expected to bring broader conversations about digital modernization.
4. **Public Safety** — Ongoing priority area including police, fire, and emergency services

## Organizational Context

### Key Contacts

- **City Manager**: Richard G. Kozal (appointed 2016; previously Assistant City Manager/COO since 2009; with city since 1995)
- **City Clerk**: Kimberly A. Dewis
- **Corporation Counsel**: Chris Beck (appointed February 2025; succeeded William Cogley after 26 years)
- **Chief Financial Officer**: Debra Nawrocki (per city financial rating information page; title is CFO, not Finance Director)
- **Community Development Director**: Marc Mylott
- **Public Services Director**: Mike Pubentz (department is titled "Public Services," not "Public Works"; manages streets, sidewalks, sewers, water mains, traffic signals, signs)
- **Water Director**: Nora Bertram
- **Assistant City Managers**: Karina Nava and Cassandra Hiller
- **Police Chief**: Ana Lalley
- **FOIA Officer**: Jennifer Quinton, Deputy Clerk, City Clerk's Office (cityclerk@elginil.gov; 847-931-5660)
- **Police Records FOIA**: Maricela Abonce, Deputy Director of Records (847-289-2575)

### Standing Committees

Elgin uses a **Committee of the Whole** model rather than traditional standing committees. The full council meets as a committee before regular sessions for discussion and deliberation. Additional boards and commissions include:

| Board/Commission | Purpose | Code Reference |
|-----------------|---------|----------------|
| Planning & Zoning Commission | Land use, zoning, development review | Ch. 3.20 |
| Board of Fire & Police Commissioners | Hiring, discipline, promotion for sworn personnel | Ch. 3.08 |
| Civilian Review Board | Police oversight | Ch. 3.10 |
| Human Relations Commission | Civil rights, fair housing, discrimination complaints | Ch. 3.12 |
| Board of Health | Public health policy and oversight | Ch. 3.32 |
| Board of Local Improvements | Special assessments for local improvements | Ch. 3.40 |
| Parks & Recreation Board | Parks and recreation policy | Ch. 3.44 |
| Police Pension Board | Police pension fund administration | Ch. 3.48 |
| Firefighters' Pension Board | Fire pension fund administration | Ch. 3.52 |
| Building Commission | Building code appeals and technical review | Ch. 3.60 |
| Electrical Committee | Electrical code technical advisory | Ch. 3.61 |
| HVAC Committee | HVAC code technical advisory | Ch. 3.62 |
| Plumbing Committee | Plumbing code technical advisory | Ch. 3.63 |
| Elgin Heritage Commission | Historic preservation review and certificates of appropriateness | Ch. 3.70 |
| Cultural Arts Commission | Arts programming and public art | Ch. 3.75 |
| Sustainability Commission | Environmental sustainability policy | Ch. 3.87 |
| Board of Examiners of Stationary Engineers | Stationary engineer licensing | Ch. 3.56 |

*Note: Some chapters in Title 3 are reserved (inactive): Civil Service Commission (3.04), Zoning & Subdivision Hearing Board (3.22), Elgin Image Advisory Commission (3.24), Hemmens Advisory Board (3.65).*

## Procedural Notes

### State Law Requirements

- **Open Meetings Act**: 5 ILCS 120 (Illinois Open Meetings Act)
- **FOIA/Public Records**: 5 ILCS 140 (Illinois Freedom of Information Act)
- **Public Hearing Requirements**: Required for zoning changes, annexations, TIF creation/extension, budget adoption; specific triggers vary by statute
- **Notice Requirements**: OMA requires 48-hour notice for regular meetings; 48-hour notice for special meetings; annual schedule posted at beginning of year

### Ethics and Disclosure

- **State Ethics Statute**: State Officials and Employees Ethics Act (5 ILCS 430)
- **Local Ethics Ordinance**: Chapter 2.86 - Ethics Act (Elgin Municipal Code, Title 2)
- **Gift Ban Threshold**: $100 cumulative per prohibited source per calendar year (exempt); $75 food/refreshments per person per single calendar day (consumed on premises). Violations: $1,001–$5,000 fine. Can cure by returning gift or donating to 501(c)(3).
- **Financial Disclosure Deadline**: May 1 annually (Illinois Statement of Economic Interests); Elgin requires duplicate filing with City Clerk and posting on city website (§2.86.050.F)
- **Ethics Officer**: Corporation Counsel serves as Ethics Officer (§2.86.110); serves as investigative authority; complaints regarding mayor, council members, city manager, corporation counsel, police chief, fire chief, or CFO are reported to city council
- **Post-Employment Restrictions**: 1-year pecuniary interest ban on contracts for former officers/full-time employees and their immediate families (§2.86.050.C); 1-year appearance ban before city boards/commissions/committees on matters personally participated in during service (§2.86.050.D)

### Local Rules

- **Rules of Procedure**: Codified in Elgin Municipal Code, Title 2, Chapter 2.08 (City Council); includes conflict of interest self-disqualification (§2.08.080.Q — "good conscience" standard) and disqualified-member vote counting (§2.08.080.R)
- **Voting Requirements**: Simple majority (5 of 9) for most actions; supermajority may be required for specific actions (e.g., emergency ordinances, override of mayor's veto under home rule)
- **Quorum**: 5 members (majority of 9-member body)

## Technology Context (Optional)

- **Current technology vendors**: [List major SaaS vendors and products]
- **IT staffing**: [Description of internal IT capacity]
- **Technology strategic plan**: [Reference or link if one exists]
- **Key integrations**: [CRM, ERP, agenda management, etc.]

## Regional Context

- **Counties**: Kane County (primary) and Cook County (Hanover Township portion)
- **Metropolitan Planning Organization**: Chicago Metropolitan Agency for Planning (CMAP)
- **Council of Governments**: Metro West Council of Governments
- **School Districts**: Elgin Area School District U-46 (unit district spanning Kane, Cook, and DuPage counties)
- **Other Taxing Bodies**: Gail Borden Public Library District, various park districts, Elgin Community College District 509, Kane County Forest Preserve District, various fire protection districts, township governments

---

*Last updated: May 27, 2026*
*Updated by: Claude (Cowork mode) with Dustin Good*
*Sources: elginil.gov official website (staff directories, FOIA page, financial rating page, agenda center), Municode municipal code structure (Titles 2, 3, 16, 18, 19; Chapter 2.86 Ethics Act), web search for TIF districts and bond ratings.*
