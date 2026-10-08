# Trustee / Conservator / Guardian / Oversight Toolkit

## 1. System Purpose

The toolkit provides a structured knowledgebase and operational interface for:

- establishing oversight responsibilities;
- identifying duties, authorities, restrictions, and requirements;
- establishing initial support or supervision;
- maintaining continuing oversight;
- reviewing decisions and actions;
- identifying uncertainty and missing information;
- evaluating sources and resources;
- documenting decisions and their provenance;
- identifying risks, liabilities, harms, and conflicts;
- mitigating identified risks;
- reviewing whether continued support remains warranted;
- managing disruptions and recovery;
- improving existing procedures;
- preserving an auditable history of actions, decisions, evidence, and changes.

The implementation below is intentionally **role-neutral**. A jurisdiction-specific implementation can add statutory requirements, court orders, agency rules, fiduciary standards, reporting requirements, and role-specific authority without changing the underlying architecture.

---

## 2. Knowledgebase Model

```text
Knowledgebase
│
├── Domains
│   ├── Authority & Scope
│   ├── Duties & Requirements
│   ├── Goals & Interests
│   ├── Needs & Conditions
│   ├── Resources & Support
│   ├── Safety & Security
│   ├── Risk & Liability
│   ├── Harm & Impact
│   ├── Reputation & Sentiment
│   ├── Sources & Evidence
│   ├── Provenance & Records
│   ├── Decisions
│   ├── Actions
│   ├── Reviews
│   ├── Communications
│   ├── Disruptions & Recovery
│   └── Process Improvement
│
├── Procedures
│   ├── Requirement Review
│   ├── Uncertainty Review
│   ├── Source Evaluation
│   ├── Resource Evaluation
│   ├── Risk Recognition
│   ├── Risk Mitigation
│   ├── Harm Review
│   ├── Liability Review
│   ├── Safety/Security Review
│   ├── Support Decision Review
│   ├── Provenance Review
│   ├── Process Establishment
│   ├── Process Improvement
│   ├── Disruption Response
│   └── Recovery
│
├── Checklists
│
├── Workflows
│
├── Decisions
│
├── Evidence
│
├── Records
│
└── Audit History
```

---

# 3. TypeScript Data Model

```tsx
export type Role =
  | "Trustee"
  | "Conservator"
  | "Guardian"
  | "Oversight"
  | "Supervisor"
  | "Other";

export type Status =
  | "not-started"
  | "in-progress"
  | "complete"
  | "blocked"
  | "needs-review";

export type Priority =
  | "critical"
  | "high"
  | "medium"
  | "low";

export type EvidenceStatus =
  | "verified"
  | "partially-verified"
  | "unverified"
  | "conflicting"
  | "missing";

export interface KnowledgeItem {
  id: string;
  title: string;
  domain: string;
  category: string;
  description: string;
  questions: string[];
  inputs: string[];
  outputs: string[];
  relatedProcedureIds: string[];
  relatedChecklistIds: string[];
  relatedWorkflowIds: string[];
}

export interface ProcedureStep {
  id: string;
  sequence: number;
  title: string;
  instruction: string;
  requiredInputs: string[];
  expectedOutputs: string[];
  decisionPoints?: string[];
}

export interface Procedure {
  id: string;
  title: string;
  purpose: string;
  applicability: string[];
  prerequisites: string[];
  steps: ProcedureStep[];
  controls: string[];
  recordsProduced: string[];
}

export interface ChecklistItem {
  id: string;
  label: string;
  description?: string;
  required: boolean;
  completed: boolean;
  evidenceRequired?: boolean;
  notes?: string;
}

export interface Checklist {
  id: string;
  title: string;
  purpose: string;
  category: string;
  applicableRoles: Role[];
  items: ChecklistItem[];
}

export interface WorkflowStep {
  id: string;
  title: string;
  type:
    | "action"
    | "decision"
    | "review"
    | "approval"
    | "escalation"
    | "record";
  description: string;
  next?: string[];
}

export interface Workflow {
  id: string;
  title: string;
  purpose: string;
  trigger: string;
  steps: WorkflowStep[];
  completionCriteria: string[];
}

export interface EvidenceRecord {
  id: string;
  title: string;
  source: string;
  date: string;
  status: EvidenceStatus;
  reliability: number;
  notes: string;
}

export interface DecisionRecord {
  id: string;
  title: string;
  date: string;
  decision: string;
  rationale: string;
  evidenceIds: string[];
  uncertainties: string[];
  risks: string[];
  reviewer?: string;
}

export interface CaseState {
  role: Role;
  subject: string;
  status: Status;
  activeChecklistIds: string[];
  activeWorkflowIds: string[];
  decisions: DecisionRecord[];
  evidence: EvidenceRecord[];
}
```

# 4. Knowledgebase

```tsx
export const knowledgebase: KnowledgeItem[] = [
  {
    id: "KB-AUTH-001",
    title: "Authority and Scope",
    domain: "Authority & Scope",
    category: "Foundational",
    description:
      "Identify the legal, organizational, contractual, delegated, or otherwise recognized basis for the oversight role and establish its boundaries.",
    questions: [
      "What authority exists?",
      "Who granted or established the authority?",
      "What activities are within scope?",
      "What activities are outside scope?",
      "What limitations apply?",
      "When does the authority begin and end?"
    ],
    inputs: [
      "appointment documents",
      "orders",
      "governing instruments",
      "policies",
      "delegations"
    ],
    outputs: [
      "authority map",
      "scope statement",
      "limitations register"
    ],
    relatedProcedureIds: ["PROC-REQ-001"],
    relatedChecklistIds: ["CHK-INITIAL-001"],
    relatedWorkflowIds: ["WF-INITIAL-001"]
  },

  {
    id: "KB-DUTY-001",
    title: "Duties, Requirements, and Limitations",
    domain: "Duties & Requirements",
    category: "Compliance",
    description:
      "Identify applicable duties, requirements, standards, prohibitions, deadlines, reporting requirements, and procedural limitations.",
    questions: [
      "What must be done?",
      "What must not be done?",
      "What approvals are required?",
      "What reporting obligations exist?",
      "What deadlines apply?"
    ],
    inputs: [
      "laws",
      "regulations",
      "orders",
      "policies",
      "professional standards"
    ],
    outputs: [
      "requirements register",
      "deadline register",
      "prohibition register"
    ],
    relatedProcedureIds: ["PROC-REQ-001"],
    relatedChecklistIds: ["CHK-REVIEW-001"],
    relatedWorkflowIds: ["WF-REVIEW-001"]
  },

  {
    id: "KB-UNC-001",
    title: "Uncertainty and Indeterminability",
    domain: "Decisions",
    category: "Uncertainty",
    description:
      "Identify facts, assumptions, interpretations, dependencies, or outcomes that remain uncertain or cannot presently be determined.",
    questions: [
      "What is known?",
      "What is unknown?",
      "What is disputed?",
      "What is inferred?",
      "What evidence is missing?",
      "What would resolve the uncertainty?"
    ],
    inputs: [
      "records",
      "observations",
      "statements",
      "evidence"
    ],
    outputs: [
      "uncertainty register",
      "verification plan",
      "qualified conclusions"
    ],
    relatedProcedureIds: ["PROC-UNC-001"],
    relatedChecklistIds: ["CHK-UNC-001"],
    relatedWorkflowIds: ["WF-UNC-001"]
  },

  {
    id: "KB-SRC-001",
    title: "Source and Evidence Evaluation",
    domain: "Sources & Evidence",
    category: "Information Quality",
    description:
      "Evaluate sources according to provenance, reliability, independence, completeness, consistency, recency, and corroboration.",
    questions: [
      "Where did the information originate?",
      "Can its provenance be established?",
      "Is the source independent?",
      "Can the claim be corroborated?",
      "Are there contradictory sources?"
    ],
    inputs: [
      "documents",
      "records",
      "communications",
      "observations",
      "source metadata"
    ],
    outputs: [
      "source assessment",
      "evidence classification",
      "confidence assessment"
    ],
    relatedProcedureIds: ["PROC-SRC-001"],
    relatedChecklistIds: ["CHK-SRC-001"],
    relatedWorkflowIds: ["WF-SRC-001"]
  },

  {
    id: "KB-RISK-001",
    title: "Risk and Liability",
    domain: "Risk & Liability",
    category: "Risk Management",
    description:
      "Identify actual and potential risks, liabilities, dependencies, failure modes, conflicts, and foreseeable adverse consequences.",
    questions: [
      "What could go wrong?",
      "Who or what could be affected?",
      "What obligations or liabilities could arise?",
      "How severe could the consequence be?",
      "How likely is the event?",
      "What controls exist?"
    ],
    inputs: [
      "activities",
      "conditions",
      "requirements",
      "known failures"
    ],
    outputs: [
      "risk register",
      "liability register",
      "controls"
    ],
    relatedProcedureIds: ["PROC-RISK-001"],
    relatedChecklistIds: ["CHK-RISK-001"],
    relatedWorkflowIds: ["WF-RISK-001"]
  },

  {
    id: "KB-HARM-001",
    title: "Actual and Potential Harm",
    domain: "Harm & Impact",
    category: "Impact Assessment",
    description:
      "Identify harms or potential harms associated with actions, omissions, decisions, conditions, risks, or liabilities.",
    questions: [
      "What harm has occurred?",
      "What harm could occur?",
      "Who could be affected?",
      "Is the harm reversible?",
      "What mitigation is available?"
    ],
    inputs: [
      "incident information",
      "risk assessments",
      "observations",
      "complaints"
    ],
    outputs: [
      "harm assessment",
      "mitigation actions",
      "escalation requirements"
    ],
    relatedProcedureIds: ["PROC-HARM-001"],
    relatedChecklistIds: ["CHK-HARM-001"],
    relatedWorkflowIds: ["WF-HARM-001"]
  },

  {
    id: "KB-SAFETY-001",
    title: "Safety and Security Requirements",
    domain: "Safety & Security",
    category: "Controls",
    description:
      "Establish appropriate safety, security, privacy, access-control, continuity, and protective requirements.",
    questions: [
      "What requires protection?",
      "From what threats?",
      "Who requires access?",
      "What access should be prohibited?",
      "What monitoring is appropriate?",
      "What emergency controls exist?"
    ],
    inputs: [
      "assets",
      "threats",
      "risks",
      "access requirements"
    ],
    outputs: [
      "security requirements",
      "access matrix",
      "emergency controls"
    ],
    relatedProcedureIds: ["PROC-SAFE-001"],
    relatedChecklistIds: ["CHK-SAFE-001"],
    relatedWorkflowIds: ["WF-SAFE-001"]
  },

  {
    id: "KB-SUPPORT-001",
    title: "Support Continuation Review",
    domain: "Support & Resources",
    category: "Continuing Oversight",
    description:
      "Determine whether initially provided or continuing support remains desirable, necessary, proportionate, effective, authorized, and appropriately controlled.",
    questions: [
      "Is support still necessary?",
      "Is the support achieving its purpose?",
      "Are less restrictive alternatives available?",
      "Has the situation changed?",
      "Is continuation authorized?",
      "Are there new risks?"
    ],
    inputs: [
      "current conditions",
      "goals",
      "outcomes",
      "risk assessments",
      "resource availability"
    ],
    outputs: [
      "continuation decision",
      "modification plan",
      "termination plan"
    ],
    relatedProcedureIds: ["PROC-SUPPORT-001"],
    relatedChecklistIds: ["CHK-SUPPORT-001"],
    relatedWorkflowIds: ["WF-SUPPORT-001"]
  },

  {
    id: "KB-PROV-001",
    title: "Provenance and Decision History",
    domain: "Provenance & Records",
    category: "Auditability",
    description:
      "Maintain lineage from source information through analysis, decision, action, review, and subsequent modification.",
    questions: [
      "Where did the information originate?",
      "What transformations occurred?",
      "Who reviewed it?",
      "What decision relied upon it?",
      "What action followed?",
      "What changed afterward?"
    ],
    inputs: [
      "source records",
      "processing history",
      "decisions",
      "actions"
    ],
    outputs: [
      "provenance graph",
      "audit trail",
      "decision history"
    ],
    relatedProcedureIds: ["PROC-PROV-001"],
    relatedChecklistIds: ["CHK-PROV-001"],
    relatedWorkflowIds: ["WF-PROV-001"]
  },

  {
    id: "KB-RECOVERY-001",
    title: "Disruption and Recovery",
    domain: "Disruptions & Recovery",
    category: "Continuity",
    description:
      "Manage minor disruptions and major failures while restoring safer and more appropriate operating conditions.",
    questions: [
      "What failed?",
      "What functions are affected?",
      "What temporary controls are required?",
      "What must be restored first?",
      "How is recovery verified?"
    ],
    inputs: [
      "incident information",
      "continuity plans",
      "system status"
    ],
    outputs: [
      "incident record",
      "recovery plan",
      "post-recovery review"
    ],
    relatedProcedureIds: ["PROC-RECOVERY-001"],
    relatedChecklistIds: ["CHK-RECOVERY-001"],
    relatedWorkflowIds: ["WF-RECOVERY-001"]
  }
];
```

# 5. Procedures

```tsx
export const procedures: Procedure[] = [
  {
    id: "PROC-REQ-001",
    title: "Requirements and Authority Review",
    purpose:
      "Establish the requirements, authority, duties, restrictions, and reporting obligations governing an activity.",
    applicability: ["initial appointment", "material change", "periodic review"],
    prerequisites: [
      "Identify subject and activity",
      "Collect governing documents"
    ],
    steps: [
      {
        id: "REQ-1",
        sequence: 1,
        title: "Identify governing sources",
        instruction:
          "Collect orders, governing instruments, laws, regulations, policies, contracts, and applicable standards.",
        requiredInputs: ["governing documents"],
        expectedOutputs: ["source inventory"]
      },
      {
        id: "REQ-2",
        sequence: 2,
        title: "Extract requirements",
        instruction:
          "Identify affirmative duties, prohibitions, approvals, deadlines, reporting requirements, and limitations.",
        requiredInputs: ["source inventory"],
        expectedOutputs: ["requirements register"]
      },
      {
        id: "REQ-3",
        sequence: 3,
        title: "Map requirements to activities",
        instruction:
          "Connect each requirement to the activity, decision, person, asset, or process to which it applies.",
        requiredInputs: ["requirements register", "activity map"],
        expectedOutputs: ["requirement-to-activity map"]
      },
      {
        id: "REQ-4",
        sequence: 4,
        title: "Identify unresolved questions",
        instruction:
          "Record ambiguities, conflicts, missing authority, uncertain interpretations, and missing documentation.",
        requiredInputs: ["requirement-to-activity map"],
        expectedOutputs: ["uncertainty register"]
      }
    ],
    controls: [
      "Do not treat an assumption as an established requirement.",
      "Record the source of every material requirement.",
      "Escalate unresolved authority conflicts."
    ],
    recordsProduced: [
      "requirements register",
      "authority map",
      "uncertainty register"
    ]
  },

  {
    id: "PROC-UNC-001",
    title: "Uncertainty Identification and Resolution",
    purpose:
      "Identify uncertainties and establish what additional information could resolve them.",
    applicability: ["decision making", "review", "dispute"],
    prerequisites: ["Relevant evidence assembled"],
    steps: [
      {
        id: "UNC-1",
        sequence: 1,
        title: "Separate known from inferred information",
        instruction:
          "Classify each material proposition as observed, documented, reported, inferred, assumed, disputed, or unknown.",
        requiredInputs: ["evidence"],
        expectedOutputs: ["knowledge classification"]
      },
      {
        id: "UNC-2",
        sequence: 2,
        title: "Identify decision-relevant uncertainties",
        instruction:
          "Determine which uncertainties could materially change the decision or action.",
        requiredInputs: ["knowledge classification"],
        expectedOutputs: ["priority uncertainty list"]
      },
      {
        id: "UNC-3",
        sequence: 3,
        title: "Define verification requirements",
        instruction:
          "Specify what evidence, observation, consultation, test, or review could resolve each uncertainty.",
        requiredInputs: ["priority uncertainty list"],
        expectedOutputs: ["verification plan"]
      }
    ],
    controls: [
      "Do not convert absence of evidence into evidence of absence.",
      "Clearly qualify conclusions affected by unresolved uncertainty."
    ],
    recordsProduced: [
      "uncertainty register",
      "verification plan"
    ]
  },

  {
    id: "PROC-SRC-001",
    title: "Source Evaluation",
    purpose:
      "Assess information sources before relying upon them for material decisions.",
    applicability: ["all material decisions"],
    prerequisites: ["Source identified"],
    steps: [
      {
        id: "SRC-1",
        sequence: 1,
        title: "Establish provenance",
        instruction:
          "Determine where the information originated and how it reached the current record.",
        requiredInputs: ["source"],
        expectedOutputs: ["provenance record"]
      },
      {
        id: "SRC-2",
        sequence: 2,
        title: "Assess reliability",
        instruction:
          "Evaluate accuracy history, independence, completeness, recency, consistency, and corroboration.",
        requiredInputs: ["provenance record"],
        expectedOutputs: ["source assessment"]
      },
      {
        id: "SRC-3",
        sequence: 3,
        title: "Identify conflicts",
        instruction:
          "Compare material claims against other available evidence.",
        requiredInputs: ["source assessment", "other evidence"],
        expectedOutputs: ["conflict assessment"]
      }
    ],
    controls: [
      "Do not treat authority as proof of every factual claim.",
      "Record conflicting evidence.",
      "Preserve the distinction between source reliability and claim validity."
    ],
    recordsProduced: [
      "source evaluation",
      "conflict assessment"
    ]
  },

  {
    id: "PROC-RISK-001",
    title: "Risk and Liability Review",
    purpose:
      "Identify foreseeable risks and liabilities and establish controls.",
    applicability: ["initial assessment", "periodic review", "material change"],
    prerequisites: ["Activity map", "requirements review"],
    steps: [
      {
        id: "RISK-1",
        sequence: 1,
        title: "Identify failure modes",
        instruction:
          "Identify actions, omissions, dependencies, conditions, or events that could produce adverse consequences.",
        requiredInputs: ["activity map"],
        expectedOutputs: ["failure-mode list"]
      },
      {
        id: "RISK-2",
        sequence: 2,
        title: "Assess impact",
        instruction:
          "Assess affected parties, severity, reversibility, duration, and scope.",
        requiredInputs: ["failure-mode list"],
        expectedOutputs: ["impact assessment"]
      },
      {
        id: "RISK-3",
        sequence: 3,
        title: "Establish controls",
        instruction:
          "Select preventive, detective, corrective, and recovery controls.",
        requiredInputs: ["impact assessment"],
        expectedOutputs: ["risk controls"]
      }
    ],
    controls: [
      "Record assumptions behind risk ratings.",
      "Review high-impact risks even when probability is uncertain.",
      "Assign responsibility for each material control."
    ],
    recordsProduced: [
      "risk register",
      "control register"
    ]
  },

  {
    id: "PROC-SUPPORT-001",
    title: "Support Continuation Review",
    purpose:
      "Determine whether continuing support or oversight remains warranted and appropriate.",
    applicability: ["periodic review", "material change", "new risk"],
    prerequisites: [
      "Current condition assessment",
      "Existing support plan"
    ],
    steps: [
      {
        id: "SUP-1",
        sequence: 1,
        title: "Reassess current conditions",
        instruction:
          "Determine whether the conditions that justified support remain present.",
        requiredInputs: ["current condition information"],
        expectedOutputs: ["current-condition assessment"]
      },
      {
        id: "SUP-2",
        sequence: 2,
        title: "Evaluate effectiveness",
        instruction:
          "Compare intended outcomes with actual outcomes.",
        requiredInputs: ["goals", "outcomes"],
        expectedOutputs: ["effectiveness assessment"]
      },
      {
        id: "SUP-3",
        sequence: 3,
        title: "Evaluate alternatives",
        instruction:
          "Identify less restrictive, more effective, safer, or otherwise preferable alternatives.",
        requiredInputs: ["effectiveness assessment"],
        expectedOutputs: ["alternative assessment"]
      },
      {
        id: "SUP-4",
        sequence: 4,
        title: "Determine continuation",
        instruction:
          "Document whether support should continue, change, pause, or terminate.",
        requiredInputs: [
          "current-condition assessment",
          "effectiveness assessment",
          "alternative assessment"
        ],
        expectedOutputs: ["continuation decision"]
      }
    ],
    controls: [
      "Do not continue support solely because it existed previously.",
      "Do not terminate support solely because an isolated problem occurred.",
      "Document the basis for continuation, modification, or termination."
    ],
    recordsProduced: [
      "support review",
      "continuation decision"
    ]
  }
];
```

# 6. Initial Checklists

```tsx
export const checklists: Checklist[] = [
  {
    id: "CHK-INITIAL-001",
    title: "Initial Appointment / Assumption of Oversight",
    purpose:
      "Establish the basic authority, scope, responsibilities, information base, and immediate controls.",
    category: "Initial Setup",
    applicableRoles: [
      "Trustee",
      "Conservator",
      "Guardian",
      "Oversight",
      "Supervisor"
    ],
    items: [
      {
        id: "I1",
        label: "Identify the subject, matter, estate, organization, or activity under oversight",
        required: true,
        completed: false
      },
      {
        id: "I2",
        label: "Obtain and preserve the governing appointment or authority document",
        required: true,
        completed: false,
        evidenceRequired: true
      },
      {
        id: "I3",
        label: "Define the scope of authority",
        required: true,
        completed: false
      },
      {
        id: "I4",
        label: "Identify restrictions and prohibited activities",
        required: true,
        completed: false
      },
      {
        id: "I5",
        label: "Identify applicable duties and reporting requirements",
        required: true,
        completed: false
      },
      {
        id: "I6",
        label: "Identify immediate safety and security requirements",
        required: true,
        completed: false
      },
      {
        id: "I7",
        label: "Identify known risks and liabilities",
        required: true,
        completed: false
      },
      {
        id: "I8",
        label: "Identify material unknowns and uncertainties",
        required: true,
        completed: false
      },
      {
        id: "I9",
        label: "Establish initial records and provenance structure",
        required: true,
        completed: false
      },
      {
        id: "I10",
        label: "Create initial review schedule",
        required: true,
        completed: false
      }
    ]
  },

  {
    id: "CHK-REVIEW-001",
    title: "Periodic Oversight Review",
    purpose:
      "Determine whether current conditions, requirements, risks, and support arrangements remain appropriate.",
    category: "Periodic Review",
    applicableRoles: [
      "Trustee",
      "Conservator",
      "Guardian",
      "Oversight",
      "Supervisor"
    ],
    items: [
      {
        id: "R1",
        label: "Review changes since the previous review",
        required: true,
        completed: false
      },
      {
        id: "R2",
        label: "Review current duties and requirements",
        required: true,
        completed: false
      },
      {
        id: "R3",
        label: "Review outstanding uncertainties",
        required: true,
        completed: false
      },
      {
        id: "R4",
        label: "Review evidence and source reliability",
        required: true,
        completed: false
      },
      {
        id: "R5",
        label: "Review current risks and liabilities",
        required: true,
        completed: false
      },
      {
        id: "R6",
        label: "Review actual or potential harms",
        required: true,
        completed: false
      },
      {
        id: "R7",
        label: "Evaluate whether current support remains warranted",
        required: true,
        completed: false
      },
      {
        id: "R8",
        label: "Identify changes required",
        required: true,
        completed: false
      },
      {
        id: "R9",
        label: "Record decisions and rationale",
        required: true,
        completed: false
      }
    ]
  },

  {
    id: "CHK-UNC-001",
    title: "Uncertainty Review",
    purpose:
      "Prevent unresolved or weakly supported assumptions from being treated as established facts.",
    category: "Decision Quality",
    applicableRoles: [
      "Trustee",
      "Conservator",
      "Guardian",
      "Oversight",
      "Supervisor"
    ],
    items: [
      {
        id: "U1",
        label: "List material propositions supporting the decision",
        required: true,
        completed: false
      },
      {
        id: "U2",
        label: "Classify each proposition as observed, documented, reported, inferred, assumed, disputed, or unknown",
        required: true,
        completed: false
      },
      {
        id: "U3",
        label: "Identify missing evidence",
        required: true,
        completed: false
      },
      {
        id: "U4",
        label: "Identify contradictory evidence",
        required: true,
        completed: false
      },
      {
        id: "U5",
        label: "Determine which uncertainties could change the decision",
        required: true,
        completed: false
      },
      {
        id: "U6",
        label: "Define verification actions",
        required: true,
        completed: false
      },
      {
        id: "U7",
        label: "Qualify conclusions that remain uncertain",
        required: true,
        completed: false
      }
    ]
  },

  {
    id: "CHK-RISK-001",
    title: "Risk, Liability, and Harm Review",
    purpose:
      "Identify and control actual and potential adverse consequences.",
    category: "Risk",
    applicableRoles: [
      "Trustee",
      "Conservator",
      "Guardian",
      "Oversight",
      "Supervisor"
    ],
    items: [
      {
        id: "K1",
        label: "Identify foreseeable failure modes",
        required: true,
        completed: false
      },
      {
        id: "K2",
        label: "Identify affected persons, assets, systems, or interests",
        required: true,
        completed: false
      },
      {
        id: "K3",
        label: "Identify existing liabilities",
        required: true,
        completed: false
      },
      {
        id: "K4",
        label: "Identify potential liabilities",
        required: true,
        completed: false
      },
      {
        id: "K5",
        label: "Identify actual harms",
        required: true,
        completed: false
      },
      {
        id: "K6",
        label: "Identify potential harms",
        required: true,
        completed: false
      },
      {
        id: "K7",
        label: "Establish mitigation controls",
        required: true,
        completed: false
      },
      {
        id: "K8",
        label: "Assign control owners",
        required: true,
        completed: false
      },
      {
        id: "K9",
        label: "Establish escalation thresholds",
        required: true,
        completed: false
      }
    ]
  }
];
```

# 7. Initial Workflows

```tsx
export const workflows: Workflow[] = [
  {
    id: "WF-INITIAL-001",
    title: "Establish Oversight",
    purpose:
      "Move from appointment or assumption of responsibility to an operationally documented oversight structure.",
    trigger: "New oversight responsibility begins",
    steps: [
      {
        id: "W1",
        title: "Establish authority",
        type: "review",
        description:
          "Identify and verify the basis and boundaries of authority.",
        next: ["W2"]
      },
      {
        id: "W2",
        title: "Map duties and restrictions",
        type: "action",
        description:
          "Create the requirements and limitations register.",
        next: ["W3"]
      },
      {
        id: "W3",
        title: "Assess immediate safety and risks",
        type: "review",
        description:
          "Identify urgent risks, liabilities, harms, and protective requirements.",
        next: ["W4"]
      },
      {
        id: "W4",
        title: "Identify uncertainties",
        type: "review",
        description:
          "Record missing, disputed, or uncertain information.",
        next: ["W5"]
      },
      {
        id: "W5",
        title: "Establish records",
        type: "record",
        description:
          "Create the initial evidence, provenance, decision, and action records.",
        next: ["W6"]
      },
      {
        id: "W6",
        title: "Activate oversight plan",
        type: "approval",
        description:
          "Confirm that the initial oversight structure is ready for operation.",
        next: []
      }
    ],
    completionCriteria: [
      "Authority established",
      "Requirements mapped",
      "Immediate risks addressed",
      "Material uncertainties documented",
      "Records established",
      "Review schedule established"
    ]
  },

  {
    id: "WF-REVIEW-001",
    title: "Periodic Oversight Review",
    purpose:
      "Conduct a structured review of the continuing oversight arrangement.",
    trigger: "Scheduled review or material change",
    steps: [
      {
        id: "PR1",
        title: "Review changes",
        type: "review",
        description:
          "Compare current conditions with the previous review.",
        next: ["PR2"]
      },
      {
        id: "PR2",
        title: "Review requirements",
        type: "review",
        description:
          "Determine whether duties, authority, or restrictions have changed.",
        next: ["PR3"]
      },
      {
        id: "PR3",
        title: "Review evidence",
        type: "review",
        description:
          "Evaluate new evidence and source reliability.",
        next: ["PR4"]
      },
      {
        id: "PR4",
        title: "Review risks and harms",
        type: "review",
        description:
          "Update risk, liability, harm, and mitigation assessments.",
        next: ["PR5"]
      },
      {
        id: "PR5",
        title: "Review support",
        type: "decision",
        description:
          "Determine whether existing support or supervision should continue.",
        next: ["PR6"]
      },
      {
        id: "PR6",
        title: "Record decision",
        type: "record",
        description:
          "Document conclusions, rationale, evidence, uncertainties, and actions.",
        next: []
      }
    ],
    completionCriteria: [
      "All material changes reviewed",
      "Risks reassessed",
      "Support reassessed",
      "Decision documented"
    ]
  },

  {
    id: "WF-SUPPORT-001",
    title: "Support Continuation / Modification / Termination",
    purpose:
      "Determine whether an existing support arrangement should continue, change, pause, or end.",
    trigger: "Periodic review or material change",
    steps: [
      {
        id: "SC1",
        title: "Establish current condition",
        type: "review",
        description:
          "Determine the present circumstances independently of historical assumptions.",
        next: ["SC2"]
      },
      {
        id: "SC2",
        title: "Measure effectiveness",
        type: "review",
        description:
          "Compare objectives with actual outcomes.",
        next: ["SC3"]
      },
      {
        id: "SC3",
        title: "Evaluate alternatives",
        type: "review",
        description:
          "Identify alternatives and their relative benefits, risks, and restrictions.",
        next: ["SC4"]
      },
      {
        id: "SC4",
        title: "Decision",
        type: "decision",
        description:
          "Select continue, modify, pause, or terminate.",
        next: ["SC5"]
      },
      {
        id: "SC5",
        title: "Document and implement",
        type: "record",
        description:
          "Record the decision, rationale, evidence, conditions, and implementation actions.",
        next: []
      }
    ],
    completionCriteria: [
      "Current condition established",
      "Effectiveness evaluated",
      "Alternatives considered",
      "Decision documented"
    ]
  },

  {
    id: "WF-RISK-001",
    title: "Risk Identification and Mitigation",
    purpose:
      "Move from recognition of a risk to documented mitigation and verification.",
    trigger: "New or changed risk",
    steps: [
      {
        id: "RK1",
        title: "Describe risk",
        type: "action",
        description:
          "Define the event, condition, or failure mode.",
        next: ["RK2"]
      },
      {
        id: "RK2",
        title: "Assess consequences",
        type: "review",
        description:
          "Identify affected interests and potential severity.",
        next: ["RK3"]
      },
      {
        id: "RK3",
        title: "Identify controls",
        type: "action",
        description:
          "Select preventive, detective, corrective, or recovery measures.",
        next: ["RK4"]
      },
      {
        id: "RK4",
        title: "Implement controls",
        type: "action",
        description:
          "Put selected controls into operation.",
        next: ["RK5"]
      },
      {
        id: "RK5",
        title: "Verify effectiveness",
        type: "review",
        description:
          "Determine whether the controls reduced the identified risk.",
        next: ["RK6"]
      },
      {
        id: "RK6",
        title: "Record residual risk",
        type: "record",
        description:
          "Document remaining risk and any required escalation.",
        next: []
      }
    ],
    completionCriteria: [
      "Risk defined",
      "Impact assessed",
      "Controls implemented",
      "Effectiveness evaluated",
      "Residual risk documented"
    ]
  }
];
```

# 8. React Application

```tsx
import React, { useMemo, useState } from "react";

type Tab =
  | "dashboard"
  | "knowledge"
  | "procedures"
  | "checklists"
  | "workflows"
  | "evidence"
  | "decisions";

export default function OversightToolkit() {
  const [tab, setTab] = useState<Tab>("dashboard");

  const [caseState, setCaseState] = useState<CaseState>({
    role: "Oversight",
    subject: "",
    status: "not-started",
    activeChecklistIds: [],
    activeWorkflowIds: [],
    decisions: [],
    evidence: []
  });

  const [selectedChecklist, setSelectedChecklist] =
    useState<Checklist | null>(null);

  const [selectedWorkflow, setSelectedWorkflow] =
    useState<Workflow | null>(null);

  const [completed, setCompleted] =
    useState<Record<string, boolean>>({});

  const activeItems = Object.values(completed).filter(Boolean).length;

  function toggleChecklistItem(id: string) {
    setCompleted(prev => ({
      ...prev,
      [id]: !prev[id]
    }));
  }

  function addChecklist(id: string) {
    if (!caseState.activeChecklistIds.includes(id)) {
      setCaseState(prev => ({
        ...prev,
        activeChecklistIds: [
          ...prev.activeChecklistIds,
          id
        ]
      }));
    }
  }

  function addWorkflow(id: string) {
    if (!caseState.activeWorkflowIds.includes(id)) {
      setCaseState(prev => ({
        ...prev,
        activeWorkflowIds: [
          ...prev.activeWorkflowIds,
          id
        ]
      }));
    }
  }

  const activeChecklists = useMemo(
    () =>
      checklists.filter(c =>
        caseState.activeChecklistIds.includes(c.id)
      ),
    [caseState.activeChecklistIds]
  );

  const activeWorkflows = useMemo(
    () =>
      workflows.filter(w =>
        caseState.activeWorkflowIds.includes(w.id)
      ),
    [caseState.activeWorkflowIds]
  );

  return (
    <div className="app">

      <header className="header">
        <div>
          <h1>Oversight Toolkit</h1>
          <p>
            Trustee · Conservator · Guardian · Oversight
            · Supervision
          </p>
        </div>

        <div className="case-controls">
          <input
            placeholder="Subject / Matter"
            value={caseState.subject}
            onChange={e =>
              setCaseState({
                ...caseState,
                subject: e.target.value
              })
            }
          />

          <select
            value={caseState.role}
            onChange={e =>
              setCaseState({
                ...caseState,
                role: e.target.value as Role
              })
            }
          >
            <option>Trustee</option>
            <option>Conservator</option>
            <option>Guardian</option>
            <option>Oversight</option>
            <option>Supervisor</option>
            <option>Other</option>
          </select>
        </div>
      </header>

      <nav className="navigation">
        {[
          ["dashboard", "Dashboard"],
          ["knowledge", "Knowledgebase"],
          ["procedures", "Procedures"],
          ["checklists", "Checklists"],
          ["workflows", "Workflows"],
          ["evidence", "Evidence"],
          ["decisions", "Decisions"]
        ].map(([key, label]) => (
          <button
            key={key}
            className={tab === key ? "active" : ""}
            onClick={() => setTab(key as Tab)}
          >
            {label}
          </button>
        ))}
      </nav>

      <main>

        {tab === "dashboard" && (
          <Dashboard
            caseState={caseState}
            activeItems={activeItems}
            activeChecklists={activeChecklists}
            activeWorkflows={activeWorkflows}
            onChecklist={id => addChecklist(id)}
            onWorkflow={id => addWorkflow(id)}
          />
        )}

        {tab === "knowledge" && (
          <Knowledgebase />
        )}

        {tab === "procedures" && (
          <ProcedureLibrary />
        )}

        {tab === "checklists" && (
          <ChecklistLibrary
            completed={completed}
            toggleItem={toggleChecklistItem}
            addChecklist={addChecklist}
            selectedChecklist={selectedChecklist}
            setSelectedChecklist={setSelectedChecklist}
          />
        )}

        {tab === "workflows" && (
          <WorkflowLibrary
            addWorkflow={addWorkflow}
            selectedWorkflow={selectedWorkflow}
            setSelectedWorkflow={setSelectedWorkflow}
          />
        )}

        {tab === "evidence" && (
          <EvidencePanel caseState={caseState} />
        )}

        {tab === "decisions" && (
          <DecisionPanel caseState={caseState} />
        )}

      </main>
    </div>
  );
}
```

# 9. Dashboard

```tsx
function Dashboard({
  caseState,
  activeItems,
  activeChecklists,
  activeWorkflows,
  onChecklist,
  onWorkflow
}: {
  caseState: CaseState;
  activeItems: number;
  activeChecklists: Checklist[];
  activeWorkflows: Workflow[];
  onChecklist: (id: string) => void;
  onWorkflow: (id: string) => void;
}) {
  return (
    <section>

      <div className="hero">
        <h2>Oversight Control Center</h2>

        <p>
          Establish, operate, review, document, and improve
          an oversight arrangement.
        </p>
      </div>

      <div className="metrics">

        <Metric
          label="Knowledge Items"
          value={knowledgebase.length}
        />

        <Metric
          label="Procedures"
          value={procedures.length}
        />

        <Metric
          label="Available Checklists"
          value={checklists.length}
        />

        <Metric
          label="Available Workflows"
          value={workflows.length}
        />

        <Metric
          label="Completed Items"
          value={activeItems}
        />

      </div>

      <div className="grid">

        <Panel title="Recommended Initial Actions">

          {[
            "Establish authority and scope",
            "Map duties and requirements",
            "Identify immediate risks",
            "Identify uncertainties",
            "Establish evidence/provenance records",
            "Create review schedule"
          ].map(item => (
            <div className="action-row" key={item}>
              <span>{item}</span>
              <span className="status-dot" />
            </div>
          ))}

        </Panel>

        <Panel title="Add Initial Checklist">

          {checklists.map(c => (
            <button
              className="library-button"
              key={c.id}
              onClick={() => onChecklist(c.id)}
            >
              + {c.title}
            </button>
          ))}

        </Panel>

        <Panel title="Add Initial Workflow">

          {workflows.map(w => (
            <button
              className="library-button"
              key={w.id}
              onClick={() => onWorkflow(w.id)}
            >
              + {w.title}
            </button>
          ))}

        </Panel>

      </div>

      <Panel title="Active Checklists">

        {activeChecklists.length === 0
          ? <EmptyState text="No active checklists." />
          : activeChecklists.map(c => (
              <div className="active-card" key={c.id}>
                {c.title}
              </div>
            ))
        }

      </Panel>

      <Panel title="Active Workflows">

        {activeWorkflows.length === 0
          ? <EmptyState text="No active workflows." />
          : activeWorkflows.map(w => (
              <div className="active-card" key={w.id}>
                {w.title}
              </div>
            ))
        }

      </Panel>

    </section>
  );
}
```

# 10. Knowledgebase UI

```tsx
function Knowledgebase() {
  const [search, setSearch] = useState("");

  const filtered = knowledgebase.filter(k =>
    `${k.title} ${k.domain} ${k.description}`
      .toLowerCase()
      .includes(search.toLowerCase())
  );

  return (
    <section>

      <SectionHeader
        title="Knowledgebase"
        description="Concepts, questions, inputs, outputs, procedures, checklists, and workflows."
      />

      <input
        className="search"
        placeholder="Search knowledgebase..."
        value={search}
        onChange={e => setSearch(e.target.value)}
      />

      <div className="knowledge-grid">

        {filtered.map(item => (

          <article className="knowledge-card" key={item.id}>

            <span className="tag">
              {item.domain}
            </span>

            <h3>{item.title}</h3>

            <p>{item.description}</p>

            <h4>Review Questions</h4>

            <ul>
              {item.questions.map(q => (
                <li key={q}>{q}</li>
              ))}
            </ul>

            <div className="relationships">
              <span>
                Procedures: {item.relatedProcedureIds.length}
              </span>

              <span>
                Checklists: {item.relatedChecklistIds.length}
              </span>

              <span>
                Workflows: {item.relatedWorkflowIds.length}
              </span>
            </div>

          </article>

        ))}

      </div>

    </section>
  );
}
```

# 11. Checklist Library

```tsx
function ChecklistLibrary({
  completed,
  toggleItem,
  addChecklist,
  selectedChecklist,
  setSelectedChecklist
}: {
  completed: Record<string, boolean>;
  toggleItem: (id: string) => void;
  addChecklist: (id: string) => void;
  selectedChecklist: Checklist | null;
  setSelectedChecklist: (c: Checklist | null) => void;
}) {
  return (
    <section>

      <SectionHeader
        title="Checklist Library"
        description="Select or add operational checklists derived from the knowledgebase."
      />

      {!selectedChecklist && (

        <div className="library-grid">

          {checklists.map(c => (

            <article className="library-card" key={c.id}>

              <h3>{c.title}</h3>

              <p>{c.purpose}</p>

              <span className="tag">
                {c.category}
              </span>

              <div className="button-row">

                <button
                  onClick={() => setSelectedChecklist(c)}
                >
                  Open
                </button>

                <button
                  onClick={() => addChecklist(c.id)}
                >
                  Add to Case
                </button>

              </div>

            </article>

          ))}

        </div>
      )}

      {selectedChecklist && (

        <ChecklistRunner
          checklist={selectedChecklist}
          completed={completed}
          toggleItem={toggleItem}
          onClose={() => setSelectedChecklist(null)}
        />

      )}

    </section>
  );
}
```

# 12. Checklist Runner

```tsx
function ChecklistRunner({
  checklist,
  completed,
  toggleItem,
  onClose
}: {
  checklist: Checklist;
  completed: Record<string, boolean>;
  toggleItem: (id: string) => void;
  onClose: () => void;
}) {

  const completedCount =
    checklist.items.filter(
      item => completed[item.id]
    ).length;

  const percentage = Math.round(
    (completedCount / checklist.items.length) * 100
  );

  return (
    <section className="runner">

      <button onClick={onClose}>
        ← Back to Library
      </button>

      <h2>{checklist.title}</h2>

      <p>{checklist.purpose}</p>

      <div className="progress">
        <div
          className="progress-bar"
          style={{ width: `${percentage}%` }}
        />
      </div>

      <p>
        {completedCount} / {checklist.items.length}
        completed
      </p>

      <div className="checklist">

        {checklist.items.map(item => (

          <label
            className={
              completed[item.id]
                ? "check-item complete"
                : "check-item"
            }
            key={item.id}
          >

            <input
              type="checkbox"
              checked={!!completed[item.id]}
              onChange={() =>
                toggleItem(item.id)
              }
            />

            <span>
              <strong>{item.label}</strong>

              {item.description && (
                <small>
                  {item.description}
                </small>
              )}

              {item.required && (
                <em>Required</em>
              )}
            </span>

          </label>

        ))}

      </div>

    </section>
  );
}
```

# 13. Workflow Library

```tsx
function WorkflowLibrary({
  addWorkflow,
  selectedWorkflow,
  setSelectedWorkflow
}: {
  addWorkflow: (id: string) => void;
  selectedWorkflow: Workflow | null;
  setSelectedWorkflow: (w: Workflow | null) => void;
}) {

  if (selectedWorkflow) {
    return (
      <WorkflowViewer
        workflow={selectedWorkflow}
        onClose={() => setSelectedWorkflow(null)}
      />
    );
  }

  return (
    <section>

      <SectionHeader
        title="Workflow Library"
        description="Operational paths derived from the knowledgebase and procedures."
      />

      <div className="workflow-grid">

        {workflows.map(workflow => (

          <article
            className="workflow-card"
            key={workflow.id}
          >

            <span className="tag">
              Workflow
            </span>

            <h3>{workflow.title}</h3>

            <p>{workflow.purpose}</p>

            <p>
              <strong>Trigger:</strong>{" "}
              {workflow.trigger}
            </p>

            <div className="workflow-count">
              {workflow.steps.length} steps
            </div>

            <div className="button-row">

              <button
                onClick={() =>
                  setSelectedWorkflow(workflow)
                }
              >
                Open
              </button>

              <button
                onClick={() =>
                  addWorkflow(workflow.id)
                }
              >
                Add to Case
              </button>

            </div>

          </article>

        ))}

      </div>

    </section>
  );
}
```

# 14. Workflow Viewer

```tsx
function WorkflowViewer({
  workflow,
  onClose
}: {
  workflow: Workflow;
  onClose: () => void;
}) {

  const [completed, setCompleted] =
    useState<Record<string, boolean>>({});

  function toggle(id: string) {
    setCompleted(prev => ({
      ...prev,
      [id]: !prev[id]
    }));
  }

  return (
    <section>

      <button onClick={onClose}>
        ← Back
      </button>

      <h2>{workflow.title}</h2>

      <p>{workflow.purpose}</p>

      <div className="workflow-path">

        {workflow.steps.map((step, index) => (

          <div className="workflow-step" key={step.id}>

            <div className="workflow-number">
              {index + 1}
            </div>

            <div className="workflow-content">

              <h3>{step.title}</h3>

              <p>{step.description}</p>

              <span className="workflow-type">
                {step.type}
              </span>

              <button
                onClick={() => toggle(step.id)}
              >
                {completed[step.id]
                  ? "Completed"
                  : "Mark Complete"}
              </button>

            </div>

          </div>

        ))}

      </div>

      <Panel title="Completion Criteria">

        <ul>
          {workflow.completionCriteria.map(
            criterion => (
              <li key={criterion}>
                {criterion}
              </li>
            )
          )}
        </ul>

      </Panel>

    </section>
  );
}
```

# 15. Procedure Library

```tsx
function ProcedureLibrary() {

  const [selected, setSelected] =
    useState<Procedure | null>(null);

  if (selected) {
    return (
      <section>

        <button onClick={() => setSelected(null)}>
          ← Back
        </button>

        <h2>{selected.title}</h2>

        <p>{selected.purpose}</p>

        {selected.steps.map(step => (

          <article
            className="procedure-step"
            key={step.id}
          >

            <span>
              Step {step.sequence}
            </span>

            <h3>{step.title}</h3>

            <p>{step.instruction}</p>

            <h4>Inputs</h4>

            <ul>
              {step.requiredInputs.map(i => (
                <li key={i}>{i}</li>
              ))}
            </ul>

            <h4>Expected Outputs</h4>

            <ul>
              {step.expectedOutputs.map(o => (
                <li key={o}>{o}</li>
              ))}
            </ul>

          </article>

        ))}

        <Panel title="Controls">
          <ul>
            {selected.controls.map(c => (
              <li key={c}>{c}</li>
            ))}
          </ul>
        </Panel>

      </section>
    );
  }

  return (
    <section>

      <SectionHeader
        title="Procedure Library"
        description="Detailed procedures supporting the toolkit."
      />

      <div className="procedure-grid">

        {procedures.map(p => (

          <article
            className="procedure-card"
            key={p.id}
          >

            <h3>{p.title}</h3>

            <p>{p.purpose}</p>

            <span>
              {p.steps.length} procedural steps
            </span>

            <button onClick={() => setSelected(p)}>
              Open Procedure
            </button>

          </article>

        ))}

      </div>

    </section>
  );
}
```

# 16. Evidence Panel

```tsx
function EvidencePanel({
  caseState
}: {
  caseState: CaseState;
}) {

  return (
    <section>

      <SectionHeader
        title="Evidence & Provenance"
        description="Maintain the information base supporting oversight decisions."
      />

      <div className="empty-panel">

        <h3>Evidence Register</h3>

        {caseState.evidence.length === 0 ? (
          <p>
            No evidence records have been entered.
          </p>
        ) : (
          caseState.evidence.map(e => (
            <article key={e.id}>
              <strong>{e.title}</strong>
              <p>{e.source}</p>
              <span>{e.status}</span>
            </article>
          ))
        )}

      </div>

      <Panel title="Evidence Evaluation Questions">

        {[
          "What is the original source?",
          "Can the provenance be established?",
          "What transformations occurred?",
          "Has the information been independently corroborated?",
          "Is contradictory evidence present?",
          "How recent is the evidence?",
          "What conclusions does the evidence actually support?"
        ].map(question => (
          <div className="question-row" key={question}>
            {question}
          </div>
        ))}

      </Panel>

    </section>
  );
}
```

# 17. Decision Panel

```tsx
function DecisionPanel({
  caseState
}: {
  caseState: CaseState;
}) {

  return (
    <section>

      <SectionHeader
        title="Decision Register"
        description="Record decisions together with their rationale, evidence, uncertainties, and risks."
      />

      {caseState.decisions.length === 0 ? (
        <EmptyState text="No decisions recorded." />
      ) : (
        caseState.decisions.map(decision => (
          <article
            className="decision-card"
            key={decision.id}
          >
            <h3>{decision.title}</h3>

            <p>
              <strong>Decision:</strong>{" "}
              {decision.decision}
            </p>

            <p>
              <strong>Rationale:</strong>{" "}
              {decision.rationale}
            </p>

            <p>
              <strong>Date:</strong>{" "}
              {decision.date}
            </p>
          </article>
        ))
      )}

    </section>
  );
}
```

# 18. Reusable UI Components

```tsx
function SectionHeader({
  title,
  description
}: {
  title: string;
  description: string;
}) {
  return (
    <header className="section-header">
      <h2>{title}</h2>
      <p>{description}</p>
    </header>
  );
}

function Panel({
  title,
  children
}: {
  title: string;
  children: React.ReactNode;
}) {
  return (
    <section className="panel">
      <h3>{title}</h3>
      {children}
    </section>
  );
}

function Metric({
  label,
  value
}: {
  label: string;
  value: number;
}) {
  return (
    <div className="metric">
      <strong>{value}</strong>
      <span>{label}</span>
    </div>
  );
}

function EmptyState({
  text
}: {
  text: string;
}) {
  return (
    <div className="empty-state">
      {text}
    </div>
  );
}
```

# 19. Styling

```css
:root {
  font-family:
    Inter,
    system-ui,
    -apple-system,
    BlinkMacSystemFont,
    "Segoe UI",
    sans-serif;

  color: #18202a;
  background: #f4f6f8;
}

* {
  box-sizing: border-box;
}

body {
  margin: 0;
}

button,
input,
select {
  font: inherit;
}

button {
  cursor: pointer;
}

.app {
  min-height: 100vh;
}

.header {
  display: flex;
  justify-content: space-between;
  gap: 24px;
  padding: 24px 32px;
  background: #17202b;
  color: white;
}

.header h1 {
  margin: 0;
}

.header p {
  margin: 6px 0 0;
  opacity: .75;
}

.case-controls {
  display: flex;
  gap: 8px;
  align-items: center;
}

.case-controls input,
.case-controls select {
  padding: 10px;
  border-radius: 6px;
  border: 1px solid #ccc;
}

.navigation {
  display: flex;
  gap: 4px;
  padding: 10px 32px;
  background: white;
  border-bottom: 1px solid #ddd;
  overflow-x: auto;
}

.navigation button {
  padding: 10px 14px;
  border: 0;
  background: transparent;
  border-radius: 6px;
}

.navigation button.active {
  background: #e7edf4;
  font-weight: 700;
}

main {
  max-width: 1400px;
  margin: auto;
  padding: 32px;
}

.hero {
  padding: 28px;
  border-radius: 12px;
  background: white;
  margin-bottom: 20px;
}

.metrics {
  display: grid;
  grid-template-columns:
    repeat(auto-fit, minmax(150px, 1fr));
  gap: 12px;
  margin-bottom: 20px;
}

.metric {
  padding: 20px;
  background: white;
  border-radius: 10px;
  display: flex;
  flex-direction: column;
  gap: 6px;
}

.metric strong {
  font-size: 28px;
}

.grid,
.knowledge-grid,
.library-grid,
.workflow-grid,
.procedure-grid {
  display: grid;
  grid-template-columns:
    repeat(auto-fit, minmax(280px, 1fr));
  gap: 16px;
  margin-bottom: 20px;
}

.panel,
.knowledge-card,
.library-card,
.workflow-card,
.procedure-card,
.decision-card,
.empty-panel {
  background: white;
  border-radius: 10px;
  padding: 20px;
  border: 1px solid #e1e5e9;
  margin-bottom: 16px;
}

.tag,
.workflow-type {
  display: inline-block;
  padding: 4px 8px;
  border-radius: 20px;
  background: #e8edf2;
  font-size: 12px;
  font-weight: 600;
}

.relationships {
  display: flex;
  gap: 10px;
  flex-wrap: wrap;
  margin-top: 16px;
  font-size: 12px;
  color: #66717e;
}

.button-row {
  display: flex;
  gap: 8px;
  margin-top: 16px;
}

.button-row button,
.library-button,
.procedure-card button,
.workflow-content button {
  border: 1px solid #cbd2d9;
  background: white;
  padding: 9px 12px;
  border-radius: 6px;
}

.button-row button:hover,
.library-button:hover {
  background: #f0f3f6;
}

.search {
  width: 100%;
  padding: 12px;
  border: 1px solid #ccd3da;
  border-radius: 8px;
  margin-bottom: 20px;
}

.check-item {
  display: flex;
  gap: 12px;
  align-items: flex-start;
  padding: 16px;
  border-bottom: 1px solid #e5e8eb;
}

.check-item input {
  margin-top: 4px;
}

.check-item span {
  display: flex;
  flex-direction: column;
  gap: 5px;
}

.check-item small {
  color: #68727d;
}

.check-item em {
  width: fit-content;
  font-size: 11px;
  font-style: normal;
  background: #f0f0f0;
  padding: 3px 6px;
  border-radius: 4px;
}

.check-item.complete {
  opacity: .6;
  text-decoration: line-through;
}

.progress {
  height: 10px;
  background: #e3e7eb;
  border-radius: 10px;
  overflow: hidden;
  margin-top: 20px;
}

.progress-bar {
  height: 100%;
  background: #4c6378;
  transition: width .2s;
}

.workflow-path {
  margin: 28px 0;
}

.workflow-step {
  display: flex;
  gap: 16px;
  margin-bottom: 16px;
}

.workflow-number {
  min-width: 36px;
  height: 36px;
  display: grid;
  place-items: center;
  border-radius: 50%;
  background: #24384b;
  color: white;
  font-weight: 700;
}

.workflow-content {
  flex: 1;
  padding: 18px;
  background: white;
  border: 1px solid #dde2e6;
  border-radius: 10px;
}

.workflow-content h3 {
  margin-top: 0;
}

.workflow-content button {
  display: block;
  margin-top: 12px;
}

.procedure-step {
  background: white;
  border-left: 4px solid #40566b;
  padding: 20px;
  margin: 12px 0;
}

.action-row,
.question-row {
  padding: 12px 0;
  border-bottom: 1px solid #e6e9ec;
}

.action-row {
  display: flex;
  justify-content: space-between;
}

.status-dot {
  width: 10px;
  height: 10px;
  border-radius: 50%;
  background: #9aa5af;
}

.active-card {
  display: inline-block;
  padding: 12px 16px;
  margin: 4px;
  border-radius: 8px;
  background: #edf2f6;
}

.empty-state {
  padding: 24px;
  text-align: center;
  color: #68727d;
}

@media (max-width: 800px) {
  .header {
    flex-direction: column;
  }

  .case-controls {
    flex-direction: column;
    align-items: stretch;
  }

  main {
    padding: 16px;
  }

  .navigation {
    padding-left: 16px;
  }
}
```

# 20. Recommended Project Structure

```text
oversight-toolkit/
│
├── src/
│   ├── App.tsx
│   ├── main.tsx
│   │
│   ├── data/
│   │   ├── knowledgebase.ts
│   │   ├── procedures.ts
│   │   ├── checklists.ts
│   │   └── workflows.ts
│   │
│   ├── models/
│   │   └── oversight.ts
│   │
│   ├── components/
│   │   ├── Dashboard.tsx
│   │   ├── Knowledgebase.tsx
│   │   ├── ProcedureLibrary.tsx
│   │   ├── ChecklistLibrary.tsx
│   │   ├── ChecklistRunner.tsx
│   │   ├── WorkflowLibrary.tsx
│   │   ├── WorkflowViewer.tsx
│   │   ├── EvidencePanel.tsx
│   │   └── DecisionPanel.tsx
│   │
│   └── styles/
│       └── oversight.css
│
├── package.json
├── tsconfig.json
└── index.html
```

# 21. Knowledgebase-to-Workflow Generation Model

The important architectural feature is that the interface does **not** have to treat checklists and workflows as isolated manually authored objects.

They can be generated or expanded from the knowledgebase.

```text
KNOWLEDGE ITEM
      │
      ├── Questions
      │       ↓
      │   REVIEW POINTS
      │
      ├── Inputs
      │       ↓
      │   INFORMATION REQUIREMENTS
      │
      ├── Outputs
      │       ↓
      │   REQUIRED RECORDS
      │
      ├── Related Procedures
      │       ↓
      │   PROCEDURAL STEPS
      │
      └── Risks / Controls
              ↓
          DECISION POINTS
              │
              ▼
        CHECKLIST / WORKFLOW
```

A future constructor can therefore use:

```tsx
function generateChecklistFromKnowledge(
  item: KnowledgeItem
): Checklist {

  return {
    id: `AUTO-${item.id}`,
    title: `${item.title} Review`,
    purpose: item.description,
    category: item.domain,
    applicableRoles: [
      "Trustee",
      "Conservator",
      "Guardian",
      "Oversight",
      "Supervisor"
    ],

    items: item.questions.map(
      (question, index) => ({
        id: `${item.id}-Q${index + 1}`,
        label: question,
        required: true,
        completed: false
      })
    )
  };
}
```

Likewise:

```tsx
function generateWorkflowFromProcedure(
  procedure: Procedure
): Workflow {

  return {
    id: `AUTO-${procedure.id}`,
    title: procedure.title,
    purpose: procedure.purpose,
    trigger: "Procedure initiated",

    steps: procedure.steps.map(step => ({
      id: step.id,
      title: step.title,
      type: "action",
      description: step.instruction,
      next: []
    })),

    completionCriteria:
      procedure.recordsProduced
        .map(record => `Record produced: ${record}`)
  };
}
```

This establishes a **knowledge → procedure → checklist/workflow → case execution → record → review** pipeline.

# 22. Oversight Case Lifecycle

```text
                    ┌────────────────────┐
                    │  Authority / Scope │
                    └─────────┬──────────┘
                              ↓
                    ┌────────────────────┐
                    │ Requirements Map   │
                    └─────────┬──────────┘
                              ↓
                    ┌────────────────────┐
                    │ Current Conditions │
                    └─────────┬──────────┘
                              ↓
                    ┌────────────────────┐
                    │ Evidence / Sources │
                    └─────────┬──────────┘
                              ↓
                    ┌────────────────────┐
                    │ Uncertainty Review │
                    └─────────┬──────────┘
                              ↓
                    ┌────────────────────┐
                    │ Risk / Harm Review │
                    └─────────┬──────────┘
                              ↓
                    ┌────────────────────┐
                    │ Decision           │
                    └─────────┬──────────┘
                              ↓
                    ┌────────────────────┐
                    │ Action / Support   │
                    └─────────┬──────────┘
                              ↓
                    ┌────────────────────┐
                    │ Monitoring / Audit │
                    └─────────┬──────────┘
                              ↓
                    ┌────────────────────┐
                    │ Periodic Review    │
                    └─────────┬──────────┘
                              │
                 ┌────────────┼────────────┐
                 ↓            ↓            ↓
              Continue      Modify       End
                 │            │            │
                 └────────────┴────────────┘
                              ↓
                         Reassessment
```

# 23. Core Record Graph

The eventual database should preserve relationships rather than merely storing isolated forms.

```text
SOURCE
  │
  ▼
EVIDENCE
  │
  ▼
PROPOSITION
  │
  ├──────────────► UNCERTAINTY
  │
  ▼
ASSESSMENT
  │
  ├──────────────► RISK
  │                   │
  │                   ▼
  │                CONTROL
  │
  ▼
DECISION
  │
  ▼
ACTION
  │
  ▼
OUTCOME
  │
  ▼
REVIEW
  │
  └──────────────► NEW DECISION
```

This allows an eventual system to answer questions such as:

- **Why was this decision made?**
- **What evidence supported it?**
- **What evidence contradicted it?**
- **What was uncertain at the time?**
- **Which requirement authorized or constrained the action?**
- **Which risks were known?**
- **Which controls were applied?**
- **What happened afterward?**
- **Who reviewed the result?**
- **What changed between reviews?**
- **Why was support continued, modified, or terminated?**

# 24. Extension Areas

The next logical knowledgebase modules are:

1. **Financial oversight**
   - assets
   - liabilities
   - expenditures
   - income
   - transactions
   - approvals
   - reconciliation
   - fraud/error controls

2. **Personal welfare / care oversight**
   - needs
   - preferences
   - goals
   - services
   - safety
   - continuity
   - complaints
   - outcomes

3. **Property / asset oversight**
   - inventory
   - condition
   - custody
   - maintenance
   - insurance
   - disposition

4. **Communications oversight**
   - incoming/outgoing communications
   - authorization
   - confidentiality
   - provenance
   - response requirements
   - escalation

5. **Incident management**
   - detection
   - classification
   - containment
   - notification
   - investigation
   - recovery
   - lessons learned

6. **Conflict-of-interest management**
   - relationship identification
   - competing interests
   - disclosure
   - recusal
   - independent review
   - mitigation

7. **Decision-quality system**
   - assumptions
   - evidence
   - uncertainty
   - competing explanations
   - alternatives
   - proportionality
   - reversibility
   - consequence analysis

8. **Provenance graph**
   - source
   - transformation
   - interpretation
   - decision
   - action
   - outcome
   - reviewer
   - timestamp

9. **Change-management system**
   - proposed change
   - reason
   - authority
   - impact
   - risk
   - approval
   - implementation
   - verification
   - rollback

10. **Periodic review engine**
    - scheduled review
    - event-triggered review
    - material-change detection
    - unresolved issue tracking
    - overdue action tracking
    - automatic checklist generation
    - decision-history comparison

# 25. Design Principle

The central design principle of the toolkit is:

```text
DO NOT BEGIN WITH:
"What should the overseer do?"

BEGIN WITH:
"What is known?
What authority exists?
What duties apply?
What is uncertain?
What interests and conditions exist?
What risks and harms are possible?
What alternatives exist?
What evidence supports each proposition?
What action is authorized and justified?
What happened afterward?
What must now be reviewed?"
```

That makes the toolkit an **oversight knowledge and decision-support system**, rather than merely a collection of forms.

The existing pseudocode/pseudoprocedure work can consequently become the **procedure layer**, while the knowledgebase becomes the **semantic layer**, and the checklists/workflows become the **operational execution layer**.