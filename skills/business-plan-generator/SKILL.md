---
name: business-plan-generator
description: "Generates comprehensive business plans with executive summary, company description, market analysis, competitive landscape, marketing strategy, operations plan, financial projections, and funding request. Includes SWOT analysis, risk assessment, and implementation timeline. Use when creating business plans, startup documentation, investor pitches, or strategic planning documents."
allowed-tools: Read, Write, Edit, WebSearch, WebFetch, AskUserQuestion, TodoWrite
---

# Business Plan Generator

Creates professional, investor-ready business plans following industry best practices and 2025 standards.

## Overview

This skill guides you through creating a comprehensive business plan that:
- Clearly communicates your business concept and value proposition
- Demonstrates thorough market research and competitive analysis
- Presents realistic financial projections backed by data
- Provides a clear roadmap for execution

## When to Use

- Starting a new business and need a formal plan
- Seeking investment or bank loans
- Creating a strategic planning document
- Documenting a business idea for stakeholders

## Workflow

### Step 1: Gather Business Information

Use AskUserQuestion to collect essential information:

**Required Information:**
1. Business name and concept (what does it do?)
2. Problem being solved (what pain point?)
3. Target customers (who will buy?)
4. Revenue model (how will you make money?)
5. Competitive advantage (why you vs competitors?)
6. Current stage (idea, MVP, revenue-generating?)
7. Funding needs (if any)
8. Team background (founders/key people)

**Store answers for use throughout the plan.**

---

### Step 2: Market Research

Conduct research using WebSearch:

```
WebSearch: "<industry>" market size trends 2025
WebSearch: "<target customer>" demographics behavior
WebSearch: "<competitor names>" business model
WebSearch: "<industry>" growth forecast
```

**Capture and organize:**
- Total Addressable Market (TAM)
- Serviceable Addressable Market (SAM)
- Serviceable Obtainable Market (SOM)
- Key industry trends
- Regulatory considerations

---

### Step 3: Competitive Analysis

Research competitors:

```
WebSearch: "<industry>" top companies competitors
WebSearch: "<competitor>" strengths weaknesses
```

**Create competitive matrix:**
| Competitor | Strengths | Weaknesses | Your Advantage |
|------------|-----------|------------|----------------|
| Competitor A | ... | ... | ... |
| Competitor B | ... | ... | ... |

---

### Step 4: SWOT Analysis

Based on research, create SWOT:

- **Strengths**: Internal advantages
- **Weaknesses**: Internal limitations
- **Opportunities**: External favorable factors
- **Threats**: External challenges

---

### Step 5: Write the Business Plan

Create the plan document with these sections:

#### 5.1 Executive Summary (Write Last)
- Business concept (2-3 sentences)
- Problem and solution
- Target market
- Business model
- Competitive advantage
- Financial highlights
- Funding request (if applicable)
- Team overview

**Keep to 1-2 pages. Write this section AFTER completing all others.**

#### 5.2 Company Description
- Mission statement
- Vision statement
- Business structure (LLC, Corp, etc.)
- Location and facilities
- History (if applicable)
- Core values

#### 5.3 Products/Services
- Detailed description
- Key features and benefits
- Pricing strategy
- Intellectual property (if any)
- Development roadmap

#### 5.4 Market Analysis
- Industry overview
- Market size (TAM/SAM/SOM)
- Market trends
- Target customer profile
- Customer needs and pain points

#### 5.5 Competitive Analysis
- Direct competitors
- Indirect competitors
- Competitive matrix
- Your competitive advantage
- Barriers to entry

#### 5.6 Marketing & Sales Strategy
- Brand positioning
- Marketing channels
- Customer acquisition strategy
- Sales process
- Customer retention strategy
- Key partnerships

#### 5.7 Operations Plan
- Business model canvas (optional)
- Key activities
- Key resources
- Supply chain (if applicable)
- Technology infrastructure
- Milestones and timeline

#### 5.8 Management Team
- Founders and key team members
- Organizational structure
- Advisors and board (if any)
- Hiring plan
- Skills gaps and how to address

#### 5.9 Financial Plan
- Revenue model
- Startup costs
- Operating expenses
- Revenue projections (3-5 years)
- Break-even analysis
- Key assumptions

**Include tables:**
- Year 1-5 Revenue Projections
- Monthly Cash Flow (Year 1)
- Profit & Loss Summary

#### 5.10 Funding Request (if applicable)
- Amount needed
- Use of funds breakdown
- Investment terms offered
- Exit strategy

#### 5.11 Risk Analysis
- Key risks identified
- Mitigation strategies
- Contingency plans

#### 5.12 Appendix
- Supporting documents
- Detailed financials
- Market research data
- Team resumes

---

### Step 6: Save the Business Plan

Ask user for preferred format and filename:

**Default:** `business-plan-<company-name>.md`

Write the complete plan to the specified file.

---

### Step 7: Review and Refine

After generating, offer to:
1. Expand any section
2. Add more market research
3. Refine financial projections
4. Improve executive summary

---

## Output

The skill produces a comprehensive business plan in Markdown format:
- 15-25 pages when rendered
- Professional structure
- Data-backed market analysis
- Realistic financial projections
- Clear executive summary

## Quality Guidelines

**DO:**
- Back claims with data and sources
- Use realistic financial projections
- Clearly identify target customers
- Acknowledge competition honestly
- Keep executive summary concise

**DON'T:**
- Pad financial projections unrealistically
- Claim "no competition exists"
- Use vague or generic language
- Skip the market research
- Make the plan overly long (>30 pages)

## Example

**Input:** "Create a business plan for a retro-style electric motorcycle company targeting urban commuters"

**Output:** A complete business plan including:
- Executive summary of the electric motorcycle concept
- Market analysis of EV and motorcycle industries
- Competitive analysis vs. existing e-motorcycle brands
- Marketing strategy targeting urban professionals
- Financial projections with unit economics
- Funding request breakdown

## Notes

- Plans are living documents - update regularly
- Financial projections should be conservative yet ambitious
- The executive summary is most critical for investors
- Include visuals and charts where helpful
