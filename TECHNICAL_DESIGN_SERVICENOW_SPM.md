# Technical Design Document (TDD)
## ServiceNow Strategic Portfolio Management (SPM) Implementation

## 1. Document Control
- **Project:** ServiceNow SPM Implementation
- **Module:** Strategic Portfolio Management
- **Version:** 1.0
- **Date:** 2026-07-07
- **Author:** Project Team

## 2. Objective
Implement ServiceNow SPM to provide end-to-end visibility and governance over demand, project, and portfolio execution. The solution will standardize intake, prioritization, planning, delivery tracking, and executive reporting.

## 3. Scope
### In Scope
- Demand Management
- Project Portfolio Management (PPM)
- Resource Management
- Financial Planning and Tracking
- Portfolio governance and reporting dashboards
- Core integrations required for planning and execution

### Out of Scope
- Full ITSM process redesign
- Custom mobile app development
- Legacy reporting platform migration outside SPM data scope

## 4. Target Architecture
### 4.1 Logical Components
- **ServiceNow SPM Application Layer**
  - Demand tables and workflows
  - Project and program records
  - Portfolio records and scorecards
  - Financial plans and cost breakdowns
  - Resource allocations and capacity views
- **Integration Layer**
  - REST/SOAP integrations via IntegrationHub/MID Server (as needed)
  - Scheduled data imports for HR, Finance, and time systems
- **Security & Access Layer**
  - Role-based access control (RBAC)
  - Data separation by business unit where required
- **Reporting Layer**
  - ServiceNow Performance Analytics and dashboards
  - Executive KPI scorecards

### 4.2 Environment Strategy
- **DEV:** Build, unit test, and peer validation
- **TEST/UAT:** End-to-end validation, user acceptance
- **PROD:** Controlled deployment via update sets/app repo pipeline

## 5. Functional Design
### 5.1 Demand Management
- Standardized demand intake form with mandatory fields:
  - Business objective
  - Expected benefit
  - Estimated cost and effort
  - Requested timeline
- Automated demand triage workflow:
  - Submission → Initial review → Scoring → Approval/Rejection
- Scoring model:
  - Strategic alignment
  - Financial value
  - Risk level
  - Regulatory impact

### 5.2 Project & Portfolio Management
- Demand-to-project conversion using governance rules.
- Project templates by delivery type (e.g., regulatory, product enhancement, infrastructure).
- Portfolio hierarchy:
  - Portfolio → Program → Project → Task
- Stage gates:
  - Initiation, Planning, Execution, Closure
- Status reporting cadence:
  - Weekly project updates
  - Monthly portfolio reviews

### 5.3 Resource Management
- Resource plans linked to project tasks and phases.
- Capacity planning by role and department.
- Allocation conflict detection and approval workflow.

### 5.4 Financial Management
- Cost model with labor and non-labor categories.
- CAPEX/OPEX tagging.
- Planned vs. actual cost tracking.
- Budget approval workflow with threshold-based approval levels.

### 5.5 Reporting & KPIs
- Portfolio health dashboard:
  - Schedule variance
  - Cost variance
  - Benefit realization status
  - Resource utilization
- Demand funnel dashboard:
  - Submitted, approved, rejected, converted

## 6. Data Design
### 6.1 Core Tables (ServiceNow Native/Extended)
- `dmn_demand` (Demand)
- `pm_project` (Project)
- `pm_program` (Program)
- `pm_portfolio` (Portfolio)
- `resource_plan` (Resource Plan)
- `fm_cost_plan` (Financial Plan)

### 6.2 Data Quality Rules
- Mandatory fields enforced on create/update.
- Reference integrity validation for portfolio-program-project links.
- Automated duplicate checks on demands by title + requesting unit.

## 7. Integration Design
### 7.1 HR System Integration
- Purpose: Sync employees and organizational units for resource planning.
- Frequency: Daily scheduled import.
- Method: REST API or file import through MID Server.

### 7.2 Finance System Integration
- Purpose: Bring actual costs for planned vs. actual reporting.
- Frequency: Daily/weekly based on finance close cycle.
- Method: IntegrationHub spoke or secure ETL feed.

### 7.3 Time Tracking Integration
- Purpose: Import approved timesheets for labor actuals.
- Frequency: Daily.

## 8. Security Design
- Role model:
  - `sn_spm.admin`
  - `sn_spm.pm`
  - `sn_spm.portfolio_manager`
  - `sn_spm.resource_manager`
  - `sn_spm.executive_viewer`
- Principle of least privilege applied to all roles.
- Sensitive financial data visibility restricted to authorized groups.
- Audit logging enabled for approval and financial changes.

## 9. Non-Functional Requirements
- **Availability:** Aligned with ServiceNow platform SLA.
- **Performance:** Standard dashboard load under 5 seconds for defined KPI widgets.
- **Scalability:** Support concurrent usage across all in-scope business units.
- **Maintainability:** Configuration-first approach; custom scripting only when required.

## 10. Implementation Approach
1. Discovery and process alignment workshops
2. Data model and workflow configuration
3. Integration build and validation
4. Reporting and KPI setup
5. SIT and UAT cycles
6. Production cutover and hypercare

## 11. Testing Strategy
- **Unit Testing:** Configuration and script validation in DEV.
- **System Integration Testing (SIT):** Workflow and integration end-to-end scenarios.
- **UAT:** Business validation of intake, approvals, planning, and reporting.
- **Regression Testing:** Key lifecycle flows before go-live.

## 12. Risks and Mitigations
- **Risk:** Incomplete upstream data quality.  
  **Mitigation:** Data validation and reconciliation reports.
- **Risk:** Low adoption due to process change.  
  **Mitigation:** Role-based training and phased rollout.
- **Risk:** Integration latency impacts reporting.  
  **Mitigation:** Monitoring alerts and retry mechanisms.

## 13. Assumptions
- Required ServiceNow SPM licenses are available.
- Integration endpoints and credentials are provisioned before SIT.
- Business owners provide KPI definitions and threshold values.

## 14. Acceptance Criteria
- End-to-end demand-to-project flow operational.
- Portfolio dashboard available to executive stakeholders.
- Financial planned vs. actual reporting validated.
- Resource capacity and allocation views functioning for target teams.
