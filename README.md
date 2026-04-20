{
  "nodes": [
    {
      "parameters": {
        "sessionIdType": "customKey",
        "sessionKey": "=={{ ($json.candidate_name || \"unknown\") + \"-\" + ($json.job || \"nojob\") }}",
        "contextWindowLength": 3
      },
      "type": "@n8n/n8n-nodes-langchain.memoryBufferWindow",
      "typeVersion": 1.3,
      "position": [
        -32,
        256
      ],
      "id": "4b67f41e-68b0-4e68-a8b6-321f408a2f22",
      "name": "Simple Memory"
    },
    {
      "parameters": {
        "operation": "append",
        "documentId": {
          "__rl": true,
          "value": "1Ov7mtwzUPWUwwOq67Gm7FbgBEdxgd2s-LBN0UtcSNrM",
          "mode": "list",
          "cachedResultName": "candidate applications",
          "cachedResultUrl": "https://docs.google.com/spreadsheets/d/1Ov7mtwzUPWUwwOq67Gm7FbgBEdxgd2s-LBN0UtcSNrM/edit?usp=drivesdk"
        },
        "sheetName": {
          "__rl": true,
          "value": "gid=0",
          "mode": "list",
          "cachedResultName": "Sheet1",
          "cachedResultUrl": "https://docs.google.com/spreadsheets/d/1Ov7mtwzUPWUwwOq67Gm7FbgBEdxgd2s-LBN0UtcSNrM/edit#gid=0"
        },
        "columns": {
          "mappingMode": "defineBelow",
          "value": {
            "Name": "={{ $json.candidate_name || '' }}",
            "Email": "={{ $json.email || '' }}",
            "Phone": "={{ $json.phone || '' }}",
            "Classification": "={{ $json.classification || '' }}",
            "Fit Score": "={{ $json.fit_score ?? 0 }}",
            "Recommendation": "={{ $json.recommendation || '' }}",
            "Risk Level": "={{ $json.risk_level || '' }}",
            "Bias Decision": "={{ $json.bias_decision || '' }}",
            "Notes": "={{ $json.notes || '' }}",
            "Job": "={{ $json.job || '' }}"
          },
          "matchingColumns": [],
          "schema": [
            {
              "id": "Name",
              "displayName": "Name",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true
            },
            {
              "id": "Email",
              "displayName": "Email",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true
            },
            {
              "id": "Phone",
              "displayName": "Phone",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true
            },
            {
              "id": "Job",
              "displayName": "Job",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true
            },
            {
              "id": "Classification",
              "displayName": "Classification",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true
            },
            {
              "id": "Fit Score",
              "displayName": "Fit Score",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true
            },
            {
              "id": "Recommendation",
              "displayName": "Recommendation",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true
            },
            {
              "id": "Risk Level",
              "displayName": "Risk Level",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true
            },
            {
              "id": "Bias Decision",
              "displayName": "Bias Decision",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true
            },
            {
              "id": "Notes",
              "displayName": "Notes",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true
            }
          ],
          "attemptToConvertTypes": false,
          "convertFieldsToString": false
        },
        "options": {}
      },
      "type": "n8n-nodes-base.googleSheets",
      "typeVersion": 4.7,
      "position": [
        880,
        544
      ],
      "id": "99e069a1-764b-4176-920a-4de55740eb04",
      "name": "Append row in sheet",
      "credentials": {
        "googleSheetsOAuth2Api": {
          "id": "IP5Qbr9JKPUMTkRY",
          "name": "Google Sheets OAuth2 API"
        }
      }
    },
    {
      "parameters": {
        "mode": "raw",
        "includeOtherFields": true,
        "options": {}
      },
      "id": "863e1652-80c1-4ae3-afd7-0a41e65133c5",
      "name": "Merge All Strategies1",
      "type": "n8n-nodes-base.set",
      "typeVersion": 3.4,
      "position": [
        528,
        544
      ]
    },
    {
      "parameters": {
        "mode": "runOnceForEachItem",
        "jsCode": "const missingSkills = Array.isArray($json.missing_skills) ? $json.missing_skills : [];\n\nconst questions = [\n  'Tell me about a time when you had to quickly learn a new tool or skill for a project.',\n  'How do you usually approach learning technical concepts you have not mastered yet?',\n  'What evidence can you share that shows you are trainable and coachable?'\n];\n\nmissingSkills.forEach(skill => {\n  questions.push(`What experience or exposure do you have with ${skill}?`);\n});\n\nreturn {\n  ...$json,\n  interview_type: 'skills_gap_interview',\n  strategy_recommendation: 'assess_trainability',\n  risk_level_final: 'medium',\n  questions,\n  next_steps: [\n    'Schedule structured interview',\n    'Assess learning agility',\n    'Recruiter reviews partial fit and growth potential'\n  ]\n};"
      },
      "id": "bbbbf105-58a2-4dfd-8660-9eea30029cd8",
      "name": "Generate Medium Candidate Strategy1",
      "type": "n8n-nodes-base.code",
      "typeVersion": 2,
      "position": [
        352,
        544
      ]
    },
    {
      "parameters": {
        "rules": {
          "values": [
            {
              "conditions": {
                "options": {
                  "caseSensitive": true,
                  "leftValue": "",
                  "typeValidation": "strict",
                  "version": 3
                },
                "conditions": [
                  {
                    "leftValue": "={{ $json.classification }}",
                    "rightValue": "high",
                    "operator": {
                      "type": "string",
                      "operation": "equals"
                    },
                    "id": "ba28fdf7-0b51-423a-ab76-275734a2ad50"
                  }
                ],
                "combinator": "and"
              }
            },
            {
              "conditions": {
                "options": {
                  "caseSensitive": true,
                  "leftValue": "",
                  "typeValidation": "strict",
                  "version": 3
                },
                "conditions": [
                  {
                    "leftValue": "={{ $json.classification }}",
                    "rightValue": "medium",
                    "operator": {
                      "type": "string",
                      "operation": "equals"
                    },
                    "id": "4d08d00c-7681-4556-91f1-506bfc3f426f"
                  }
                ],
                "combinator": "and"
              }
            },
            {
              "conditions": {
                "options": {
                  "caseSensitive": true,
                  "leftValue": "",
                  "typeValidation": "strict",
                  "version": 3
                },
                "conditions": [
                  {
                    "leftValue": "={{ $json.classification }}",
                    "rightValue": "high_risk",
                    "operator": {
                      "type": "string",
                      "operation": "equals"
                    },
                    "id": "d63784c4-0834-40bb-acc0-c0b30af46b40"
                  }
                ],
                "combinator": "and"
              }
            }
          ]
        },
        "options": {}
      },
      "id": "979c5c29-abf6-441b-8cff-74bd9b566565",
      "name": "Route by Classification1",
      "type": "n8n-nodes-base.switch",
      "typeVersion": 3.4,
      "position": [
        176,
        528
      ]
    },
    {
      "parameters": {
        "promptType": "define",
        "text": "=Candidate: {{ $json.candidate_name }}\nTarget Job: {{ $json.job }}\nFit Score: {{ $json.fit_score }}\nConfidence Score: {{ $json.confidence_score }}\nRecommendation: {{ $json.recommendation }}\nReasoning: {{ $json.reasoning }}\nStrengths: {{ Array.isArray($json.strengths) ? $json.strengths.join(', ') : '' }}\nWeaknesses: {{ Array.isArray($json.weaknesses) ? $json.weaknesses.join(', ') : '' }}\nMissing Skills: {{ Array.isArray($json.missing_skills) ? $json.missing_skills.join(', ') : '' }}\nAgent 1 Confidence: {{ $json.agent1_confidence }}\nAgent 1 Notes: {{ $json.agent1_notes }}\n\nAudit this hiring recommendation for fairness.\n\nImportant:\n- Missing information alone is not a fairness violation.\n- Moderate uncertainty alone is not a fairness violation.\n- Only FLAG clear fairness problems or major contradictions between evidence and recommendation.\n\nReturn ONLY valid JSON:\n{\n  \"decision\": \"PASS\" or \"FLAG\",\n  \"reason\": \"short explanation\",\n  \"risk_level\": \"low\" or \"medium\" or \"high\"\n}",
        "options": {
          "systemMessage": "You are Agent 3: Bias and Fairness Auditor Agent.\n\nYour job is to audit the recommendation for fairness issues, not to punish normal uncertainty.\n\nReturn PASS unless there is a clear fairness concern or clearly unsupported extreme decision.\n\nUse FLAG only when at least one of these is true:\n- the candidate appears to be judged using non-job-relevant factors\n- the candidate is strongly rejected despite moderate or high fit evidence\n- the decision contradicts the evidence in a major way\n- the reasoning is clearly unfair or clearly unsupported\n\nDo NOT return FLAG only because:\n- some information is missing\n- the resume is incomplete\n- the candidate may be trainable\n- confidence is moderate rather than high\n\nRisk level rules:\n- low = recommendation is fair and evidence is reasonably aligned\n- medium = some uncertainty or incomplete evidence, but no fairness issue\n- high = strong fairness concern or major evidence contradiction\n\nReturn ONLY valid JSON."
        }
      },
      "id": "748e1600-1e49-4502-92fa-b5cadba6d01d",
      "name": "Bias Detection Agent1",
      "type": "@n8n/n8n-nodes-langchain.agent",
      "typeVersion": 3,
      "position": [
        -240,
        544
      ]
    },
    {
      "parameters": {
        "text": "={{ $json.resume_text }}",
        "schemaType": "fromJson",
        "jsonSchemaExample": "{\n  \"candidate_name\": \"Jane Doe\",\n  \"contact_info\": \"jane@email.com | 1234567890\",\n  \"skills\": [\"Python\", \"SQL\", \"Data Analysis\"],\n  \"experience\": [{\"role\": \"Data Analyst\", \"company\": \"Example Corp\", \"duration\": \"2021-2024\", \"confidence\": 0.9}],\n  \"education\": [\"B.S. in Statistics\"],\n  \"certifications\": [\"Google Data Analytics Certificate\"],\n  \"seniority_level\": \"mid\",\n  \"employment_gaps\": \"None detected\",\n  \"flags\": [],\n  \"confidence_summary\": {\n    \"overall_confidence\": 0.9,\n    \"notes\": \"Candidate information was clearly stated in the resume.\"\n  }\n}",
        "options": {
          "systemPromptTemplate": "You are Agent 1: Candidate Understanding Agent.\n\nAnalyze the resume text and return a structured JSON object.\n\nYou MUST return ALL of the following fields:\n- candidate_name (string)\n- contact_info (string)\n- skills (array)\n- experience (array)\n- education (array)\n- certifications (array)\n- seniority_level (string)\n- employment_gaps (string)\n- flags (array of strings)\n- confidence_summary (object)\n\nconfidence_summary must include:\n- overall_confidence (number between 0 and 1)\n- notes (string)\n\nRules:\n- Always return valid JSON\n- Never leave fields empty; use best effort\n- If information is missing, explain why in notes\n- flags should mention missing or unclear information\n- Return ONLY JSON"
        }
      },
      "id": "425a140c-2fa7-43d1-83cb-a3a119482c5e",
      "name": "Candidate Understanding Agent1",
      "type": "@n8n/n8n-nodes-langchain.informationExtractor",
      "typeVersion": 1,
      "position": [
        -1136,
        560
      ]
    },
    {
      "parameters": {
        "jsCode": "const prev = $node[\"Parse Agent 3 Output\"].json;\n\nreturn [{\n  json: {\n    candidate_name: prev.candidate_name || \"\",\n    email: prev.email || \"\",\n    phone: prev.phone || \"\",\n    job: prev.job || \"\",\n    classification: $json.classification || \"\",\n    fit_score: prev.fit_score ?? 0,\n    recommendation: prev.recommendation || \"\",\n    risk_level: prev.risk_level || \"\",\n    bias_decision: prev.bias_decision || \"\",\n    notes: prev.agent1_notes || prev.reasoning || \"\"\n  }\n}];"
      },
      "id": "ccab5d1c-0185-456d-b2a6-00828c06afad",
      "name": "Prepare Final Output",
      "type": "n8n-nodes-base.code",
      "typeVersion": 2,
      "position": [
        704,
        544
      ]
    },
    {
      "parameters": {
        "jsCode": "const raw = $json.output ? JSON.parse($json.output) : {};\nconst prev = $node[\"Parse Agent 2 Output1\"].json;\n\nreturn [{\n  json: {\n    candidate_name: prev.candidate_name || \"\",\n    email: prev.email || \"\",\n    phone: prev.phone || \"\",\n    job: prev.job || \"\",\n\n    skills: prev.skills || [],\n    experience: prev.experience || [],\n    education: prev.education || [],\n    certifications: prev.certifications || [],\n    employment_gaps: prev.employment_gaps || \"\",\n    flags: prev.flags || [],\n\n    fit_score: prev.fit_score ?? 0,\n    confidence_score: prev.confidence_score ?? 0,\n    recommendation: prev.recommendation || \"\",\n    reasoning: prev.reasoning || \"\",\n    strengths: prev.strengths || [],\n    weaknesses: prev.weaknesses || [],\n    missing_skills: prev.missing_skills || [],\n\n    agent1_confidence: prev.agent1_confidence || \"\",\n    agent1_notes: prev.agent1_notes || \"\",\n\n    bias_decision: raw.decision || \"\",\n    bias_reason: raw.reason || \"\",\n    risk_level: raw.risk_level || \"low\",\n    bias_flag: (raw.decision || \"\") === \"FLAG\"\n  }\n}];"
      },
      "id": "ac3a37f7-a639-4fa1-a1f5-3a8c8228b8d5",
      "name": "Parse Agent 3 Output",
      "type": "n8n-nodes-base.code",
      "typeVersion": 2,
      "position": [
        16,
        544
      ]
    },
    {
      "parameters": {
        "model": {
          "__rl": true,
          "mode": "list",
          "value": "gpt-5-mini"
        },
        "builtInTools": {},
        "options": {}
      },
      "id": "2c60df5d-3ea1-4db5-bd7f-003c788ad67f",
      "name": "OpenAI Chat Model1",
      "type": "@n8n/n8n-nodes-langchain.lmChatOpenAi",
      "typeVersion": 1.3,
      "position": [
        -1120,
        672
      ],
      "credentials": {
        "openAiApi": {
          "id": "QiCTKQmSXQvHKWVq",
          "name": "n8n free OpenAI API credits"
        }
      }
    },
    {
      "parameters": {
        "httpMethod": "POST",
        "path": "1c8dfa91-cf76-41b6-8b82-981cbe20e295",
        "responseMode": "responseNode",
        "options": {}
      },
      "id": "14ae89af-ab76-471a-932b-22ac6cd7c33b",
      "name": "Webhook1",
      "type": "n8n-nodes-base.webhook",
      "typeVersion": 2,
      "position": [
        -1424,
        560
      ],
      "webhookId": "1c8dfa91-cf76-41b6-8b82-981cbe20e295"
    },
    {
      "parameters": {
        "content": "## **Agent 3**",
        "height": 80,
        "width": 192
      },
      "type": "n8n-nodes-base.stickyNote",
      "position": [
        -240,
        464
      ],
      "typeVersion": 1,
      "id": "120d6b2a-d41b-4ea0-ba76-d76fed71812e",
      "name": "Sticky Note14"
    },
    {
      "parameters": {
        "content": "## **Agent 4**",
        "height": 80
      },
      "type": "n8n-nodes-base.stickyNote",
      "position": [
        -112,
        32
      ],
      "typeVersion": 1,
      "id": "acf2ec54-dda6-496f-838d-97bcb23fcc73",
      "name": "Sticky Note6"
    },
    {
      "parameters": {
        "mode": "runOnceForEachItem",
        "jsCode": "const questions = [\n  'Can you provide concrete examples that verify your claimed skills and experience?',\n  'What projects best demonstrate your readiness for this role?',\n  'How do you respond to feedback about skill gaps or inconsistencies?',\n  'Who can verify your prior work and responsibilities?'\n];\n\nreturn {\n  ...$json,\n  interview_type: 'verification_screen',\n  strategy_recommendation: 'human_review_or_caution',\n  risk_level_final: 'high',\n  questions,\n  next_steps: [\n    'Pause automatic advancement',\n    'Recruiter reviews fairness flag or low-confidence result',\n    'Use manual screening before interview decision'\n  ]\n};"
      },
      "id": "e58377ee-42b9-43c1-bc77-d49e2c556fc8",
      "name": "Generate High Risk Strategy",
      "type": "n8n-nodes-base.code",
      "typeVersion": 2,
      "position": [
        352,
        720
      ]
    },
    {
      "parameters": {
        "mode": "runOnceForEachItem",
        "jsCode": "const skills = Array.isArray($json.strengths) ? $json.strengths : [];\nconst missingSkills = Array.isArray($json.missing_skills) ? $json.missing_skills : [];\n\nconst questions = [\n  {\n    question: 'Describe a complex technical challenge you faced in a recent project. How did you approach it and what was the outcome?',\n    focus_area: 'problem_solving',\n    difficulty: 'advanced'\n  },\n  {\n    question: `Walk us through your experience with ${skills[0] || 'your strongest technical skill'}. Give a concrete business example.`,\n    focus_area: 'technical_depth',\n    difficulty: 'advanced'\n  },\n  {\n    question: 'How do you communicate technical findings to non-technical stakeholders?',\n    focus_area: 'communication',\n    difficulty: 'intermediate'\n  }\n];\n\nif (missingSkills.length > 0) {\n  questions.push({\n    question: `We noticed ${missingSkills[0]} may be less developed. How would you get up to speed quickly if hired?`,\n    focus_area: 'adaptability',\n    difficulty: 'intermediate'\n  });\n}\n\nreturn {\n  ...$json,\n  interview_type: 'advanced_technical',\n  strategy_recommendation: 'proceed_with_advanced_interview',\n  risk_level_final: 'low',\n  questions,\n  next_steps: [\n    'Schedule advanced technical interview',\n    'Validate technical depth with examples',\n    'Move to final recruiter review'\n  ]\n};"
      },
      "id": "572ef967-843c-46f6-b55d-43598301f469",
      "name": "Generate High Candidate Strategy",
      "type": "n8n-nodes-base.code",
      "typeVersion": 2,
      "position": [
        352,
        368
      ]
    },
    {
      "parameters": {
        "model": {
          "__rl": true,
          "mode": "list",
          "value": "gpt-5-mini"
        },
        "builtInTools": {},
        "options": {}
      },
      "id": "82c3a26e-e4f1-4706-abb2-6fa7f50c5390",
      "name": "OpenAI Chat Model6",
      "type": "@n8n/n8n-nodes-langchain.lmChatOpenAi",
      "typeVersion": 1.3,
      "position": [
        -256,
        688
      ],
      "credentials": {
        "openAiApi": {
          "id": "QiCTKQmSXQvHKWVq",
          "name": "n8n free OpenAI API credits"
        }
      }
    },
    {
      "parameters": {
        "content": "## Agent 1 \n",
        "height": 96,
        "width": 150
      },
      "type": "n8n-nodes-base.stickyNote",
      "position": [
        -1120,
        432
      ],
      "typeVersion": 1,
      "id": "69cc5137-16a4-4bc5-8f64-d88f3645550e",
      "name": "Sticky Note1"
    },
    {
      "parameters": {
        "jsCode": "const raw = $json.output ? JSON.parse($json.output) : {};\n\nreturn [{\n  json: {\n    ...$json,\n    classification: raw.classification || \"medium\",\n    classification_reason: raw.classification_reason || \"\",\n    should_advance: raw.should_advance ?? false,\n    interview_questions: Array.isArray(raw.interview_questions) ? raw.interview_questions : []\n  }\n}];"
      },
      "type": "n8n-nodes-base.code",
      "typeVersion": 2,
      "position": [
        240,
        128
      ],
      "id": "6eb15aa8-ef60-4744-9a74-49d6608e6a0b",
      "name": "Code in JavaScript3"
    },
    {
      "parameters": {
        "model": {
          "__rl": true,
          "mode": "list",
          "value": "gpt-5-mini"
        },
        "builtInTools": {},
        "options": {}
      },
      "type": "@n8n/n8n-nodes-langchain.lmChatOpenAi",
      "typeVersion": 1.3,
      "position": [
        -160,
        224
      ],
      "id": "40a863a6-38e2-42cb-9e01-0a8935505432",
      "name": "OpenAI Chat Model (Agent 4)1",
      "credentials": {
        "openAiApi": {
          "id": "QiCTKQmSXQvHKWVq",
          "name": "n8n free OpenAI API credits"
        }
      }
    },
    {
      "parameters": {
        "promptType": "define",
        "text": "=Candidate: {{ $json.candidate_name }}\nTarget Job: {{ $json.job }}\n\nSkills: {{ Array.isArray($json.skills) ? $json.skills.join(', ') : '' }}\nExperience: {{ JSON.stringify($json.experience || []) }}\nEducation: {{ Array.isArray($json.education) ? $json.education.join(', ') : '' }}\nCertifications: {{ Array.isArray($json.certifications) ? $json.certifications.join(', ') : '' }}\nEmployment Gaps: {{ $json.employment_gaps }}\nFlags from Agent 1: {{ Array.isArray($json.flags) ? $json.flags.join(', ') : '' }}\nAgent 1 Notes: {{ $json.agent1_notes }}\n\nFit Score: {{ $json.fit_score }}\nConfidence Score: {{ $json.confidence_score }}\nRecommendation: {{ $json.recommendation }}\nReasoning: {{ $json.reasoning }}\nStrengths: {{ Array.isArray($json.strengths) ? $json.strengths.join(', ') : '' }}\nWeaknesses: {{ Array.isArray($json.weaknesses) ? $json.weaknesses.join(', ') : '' }}\nMissing Skills: {{ Array.isArray($json.missing_skills) ? $json.missing_skills.join(', ') : '' }}\n\nBias Decision: {{ $json.bias_decision }}\nBias Reason: {{ $json.bias_reason }}\nRisk Level: {{ $json.risk_level }}\n\nClassify this candidate into one of these categories:\n- high\n- medium\n- high_risk\n\nThen decide whether the candidate should advance to an interview.\n\nIf the candidate should advance, generate 3 to 5 interview questions for HR based on:\n- the candidate’s experience\n- strengths\n- missing skills\n- areas needing clarification\n\nRules:\n- high = strong fit, ready to move forward\n- medium = partial fit, may advance with clarification\n- high_risk = do not advance unless manual review is needed\n- interview questions must be practical, job-related, and specific to the candidate\n- if candidate should not advance, return an empty interview_questions array\n\nReturn ONLY valid JSON:\n\n{\n  \"classification\": \"high\" | \"medium\" | \"high_risk\",\n  \"classification_reason\": \"short explanation\",\n  \"should_advance\": true | false,\n  \"interview_questions\": [\"...\", \"...\", \"...\"]\n}",
        "options": {
          "systemMessage": "You are Agent 4: Candidate Classification and Interview Prep Agent.\n\nIf classification is high_risk, do not leave interview_questions empty.\nInstead return 3 to 5 recruiter review or screening questions for manual follow-up.\n\nYou may use memory to check whether this candidate has been reviewed before for the same session key.\nUse memory only as supporting context.\nAlways prioritize the current evidence from Agents 1, 2, and 3.\n\nYour job is to:\n1. classify the candidate\n2. decide whether the candidate should advance\n3. generate interview questions for HR if the candidate advances\n\nRules:\n- Return ONLY valid JSON\n- classification must be exactly one of: high, medium, high_risk\n- should_advance must be true or false\n- interview_questions must be an array\n- If the candidate should not advance, return an empty interview_questions array\n- Questions must be job-relevant, professional, and based on the candidate’s actual background"
        }
      },
      "type": "@n8n/n8n-nodes-langchain.agent",
      "typeVersion": 3.1,
      "position": [
        -160,
        80
      ],
      "id": "e4b9767d-d012-4c80-b812-5587e52c3804",
      "name": "AI Agent1"
    },
    {
      "parameters": {
        "conditions": {
          "options": {
            "caseSensitive": true,
            "leftValue": "",
            "typeValidation": "loose",
            "version": 3
          },
          "conditions": [
            {
              "id": "ffab4696-53e8-41e3-bb43-e64abfc25679",
              "leftValue": "=={{ $json.should_advance }}",
              "rightValue": "",
              "operator": {
                "type": "boolean",
                "operation": "true",
                "singleValue": true
              }
            }
          ],
          "combinator": "and"
        },
        "looseTypeValidation": true,
        "options": {}
      },
      "type": "n8n-nodes-base.if",
      "typeVersion": 2.3,
      "position": [
        448,
        160
      ],
      "id": "e26c9a71-3e47-435d-a170-1ccc6d7f3ade",
      "name": "If1"
    },
    {
      "parameters": {
        "content": "## **Agent 2**",
        "height": 80,
        "width": 150
      },
      "type": "n8n-nodes-base.stickyNote",
      "position": [
        -736,
        864
      ],
      "typeVersion": 1,
      "id": "516a7e36-e860-4b57-965e-f62d1038c0a3",
      "name": "Sticky Note12"
    },
    {
      "parameters": {
        "model": {
          "__rl": true,
          "mode": "list",
          "value": "gpt-5-mini"
        },
        "builtInTools": {},
        "options": {}
      },
      "id": "69c4289c-bff8-4678-af8a-f983a1514392",
      "name": "OpenAI Chat Model3",
      "type": "@n8n/n8n-nodes-langchain.lmChatOpenAi",
      "typeVersion": 1.3,
      "position": [
        -704,
        720
      ],
      "credentials": {
        "openAiApi": {
          "id": "QiCTKQmSXQvHKWVq",
          "name": "n8n free OpenAI API credits"
        }
      }
    },
    {
      "parameters": {
        "jsCode": "const raw = $json.output ? JSON.parse($json.output) : {};\n\nreturn [{\n  json: {\n    candidate_name: $node[\"Code in JavaScript1\"].json[\"candidate_name\"] || \"\",\n    job: $node[\"Code in JavaScript1\"].json[\"job\"] || \"\",\n    fit_score: raw.fit_score ?? 0,\n    confidence_score: raw.confidence_score ?? 0,\n    recommendation: raw.recommendation || \"\",\n    reasoning: raw.reasoning || \"\",\n    strengths: Array.isArray(raw.strengths) ? raw.strengths : [],\n    weaknesses: Array.isArray(raw.weaknesses) ? raw.weaknesses : [],\n    missing_skills: Array.isArray(raw.missing_skills) ? raw.missing_skills : [],\n    agent1_confidence: $node[\"Code in JavaScript1\"].json[\"confidence_from_agent1\"] || \"\",\n    agent1_notes: $node[\"Code in JavaScript1\"].json[\"agent1_notes\"] || \"\",\n    email: $node[\"Code in JavaScript1\"].json[\"email\"] || \"\",\n    phone: $node[\"Code in JavaScript1\"].json[\"phone\"] || \"\"\n  }\n}];"
      },
      "id": "5edbb488-7d29-4be9-b449-d804b4c22e75",
      "name": "Parse Agent 2 Output1",
      "type": "n8n-nodes-base.code",
      "typeVersion": 2,
      "position": [
        -432,
        560
      ]
    },
    {
      "parameters": {
        "promptType": "define",
        "text": "=Candidate: {{ $json.candidate_name }}\nTarget Job: {{ $json.job }}\nSkills: {{ $json.skills }}\nEducation: {{ $json.education }}\nExperience: {{ $json.experience }}\nCertifications: {{ $json.certifications }}\nEmployment Gaps: {{ $json.employment_gaps }}\nFlags from Agent 1: {{ $json.flags }}\nAgent 1 Confidence: {{ $json.confidence_from_agent1 }}\nAgent 1 Notes: {{ $json.agent1_notes }}\n\nImportant instruction:\nAgent 1 is responsible for judging the quality and reliability of the resume data.\nIf Agent 1 confidence is low or Agent 1 notes indicate incomplete evidence, you must be cautious.\nIn low-confidence cases, avoid strong recommendations and prefer \"review\" unless there is very strong evidence.\n\nEvaluate this candidate for the target job and return ONLY JSON:\n{\n  \"fit_score\": number (0-100),\n  \"confidence_score\": number (0-1),\n  \"recommendation\": \"advance\" | \"review\" | \"reject\",\n  \"reasoning\": \"short explanation\",\n  \"missing_skills\": [\"...\"],\n  \"strengths\": [\"...\"],\n  \"weaknesses\": [\"...\"]\n}",
        "options": {
          "systemMessage": "=You are Agent 2: Job Fit Evaluation Agent.\n\nWhen evaluating a candidate's fit, first use the technical_knowledge_base tool to retrieve the gold-standard requirements for the target role. Compare the candidate's skills against these retrieved standards to calculate the Fit Score.\n\nEvaluate candidates objectively based on skills, experience, education, and certifications.\n\nDo NOT use bias factors such as name, gender, ethnicity, age, or similar personal assumptions.\n\nYou must also respect Agent 1's confidence and notes.\nAgent 1 evaluates the quality and reliability of the resume data.\nIf Agent 1 confidence is low, you must be cautious about making strong recommendations.\n\nRules:\n- fit_score must be 0 to 100\n- confidence_score must be 0 to 1\n- recommendation must be exactly one of: advance, review, reject\n- If Agent 1 confidence is below 0.5, do not return \"advance\" unless there is exceptionally strong evidence\n- If Agent 1 confidence is below 0.5, reduce confidence_score and prefer \"review\"\n- If Agent 1 notes or flags show missing or incomplete evidence, reflect that in reasoning, weaknesses, and recommendation\n- return ONLY valid JSON"
        }
      },
      "id": "ba3f32ce-87b9-4ace-aeda-227607047fcc",
      "name": "AI Agent2",
      "type": "@n8n/n8n-nodes-langchain.agent",
      "typeVersion": 3,
      "position": [
        -720,
        560
      ]
    },
    {
      "parameters": {
        "jsCode": "function toList(value) {\n  if (!value) return \"\";\n\n  if (Array.isArray(value)) {\n    return value.join(\", \");\n  }\n\n  if (typeof value === \"object\") {\n    return Object.values(value).join(\", \");\n  }\n\n  return String(value);\n}\n\nconst src = $json.output || {};\nconst intake = $node[\"prepare webhoook input\"].json || {};\n\nreturn [\n  {\n    json: {\n      candidate_name: src.candidate_name || \"\",\n      skills: toList(src.skills),\n      education: toList(src.education),\n      experience: JSON.stringify(src.experience || []),\n      certifications: toList(src.certifications),\n      employment_gaps: src.employment_gaps || \"\",\n      flags: toList(src.flags),\n      confidence_from_agent1: String(src.confidence_summary?.overall_confidence || \"\"),\n      agent1_notes: String(src.confidence_summary?.notes || \"\"),\n      job: intake.job || \"\",\n      email: intake.email || \"\",\n      phone: intake.phone || \"\"\n    }\n  }\n];"
      },
      "type": "n8n-nodes-base.code",
      "typeVersion": 2,
      "position": [
        -864,
        560
      ],
      "id": "e2015c15-dab8-41ac-ae72-b2c28802390c",
      "name": "Code in JavaScript1"
    },
    {
      "parameters": {
        "options": {}
      },
      "type": "@n8n/n8n-nodes-langchain.embeddingsOpenAi",
      "typeVersion": 1.2,
      "position": [
        -592,
        960
      ],
      "id": "d0c98253-40f2-41c6-ba46-1681ccbd3652",
      "name": "Embeddings OpenAI",
      "credentials": {
        "openAiApi": {
          "id": "QiCTKQmSXQvHKWVq",
          "name": "n8n free OpenAI API credits"
        }
      }
    },
    {
      "parameters": {
        "mode": "retrieve-as-tool",
        "toolDescription": "Use this tool to look up technical skill definitions, industry standards, and job requirement details to verify candidate fit.",
        "memoryKey": {
          "__rl": true,
          "value": "job_fit_data",
          "mode": "list"
        }
      },
      "type": "@n8n/n8n-nodes-langchain.vectorStoreInMemory",
      "typeVersion": 1.3,
      "position": [
        -544,
        784
      ],
      "id": "aa8cacdc-b7b0-4518-b195-0e49f5fe0298",
      "name": "Simple Vector Store1"
    },
    {
      "parameters": {
        "jsCode": "let payload = {};\n\nif (typeof $json.body === \"string\") {\n  try {\n    payload = JSON.parse($json.body);\n  } catch {\n    payload = {};\n  }\n} else if ($json.body && typeof $json.body === \"object\") {\n  payload = $json.body;\n} else if ($json.query && typeof $json.query === \"object\") {\n  payload = $json.query;\n}\n\nreturn [\n  {\n    json: {\n      name: payload.name || \"\",\n      email: payload.email || \"\",\n      phone: payload.phone || \"\",\n      job: payload.job || \"\",\n      message: payload.message || \"\",\n      resume_text: payload.resume_text || \"\"\n    }\n  }\n];"
      },
      "type": "n8n-nodes-base.code",
      "typeVersion": 2,
      "position": [
        -1280,
        560
      ],
      "id": "9930b64d-7e72-4411-9225-254807f5442f",
      "name": "prepare webhoook input"
    },
    {
      "parameters": {
        "sendTo": "isabellalomb2004@gmail.com",
        "subject": "={{ \"Application Update for \" + (($json[\"Job\"] || $json.job) || \"Your Application\") }}",
        "message": "={{\n\"Hello \" + (($json[\"Name\"] || $json.candidate_name) || \"Applicant\") + \",\\n\\n\" +\n\"Thank you for applying for the \" + (($json[\"Job\"] || $json.job) || \"position\") + \". After review, we will not be moving forward with your application at this time.\\n\\n\" +\n\"We appreciate your interest and the time you took to apply.\\n\\n\" +\n\"Best regards,\\nHR Team\"\n}}",
        "options": {}
      },
      "type": "n8n-nodes-base.gmail",
      "typeVersion": 2.2,
      "position": [
        1552,
        624
      ],
      "id": "bf9d8864-8c90-44a9-8bce-93052eed1cb1",
      "name": "Reject email",
      "webhookId": "d943914e-d5fe-440c-87a0-3a617a0ca0e4",
      "credentials": {
        "gmailOAuth2": {
          "id": "nJ2aKvguIpwbeuEN",
          "name": "Gmail account"
        }
      }
    },
    {
      "parameters": {
        "sendTo": "isabellalomb2004@gmail.com",
        "subject": "={{ \"Interview Scheduled for \" + (($json[\"Job\"] || $json.job) || \"Your Application\") }}",
        "message": "={{\n\"Hello \" + (($json[\"Name\"] || $json.candidate_name) || \"Applicant\") + \",\\n\\n\" +\n\"Congratulations. You have been selected to move forward in the hiring process for the \" + (($json[\"Job\"] || $json.job) || \"position\") + \".\\n\\n\" +\n\"Your interview has been scheduled for Monday, April 27, 2026 at 10:00 AM.\\n\\n\" +\n\"Please reply to this email if you have any questions.\\n\\n\" +\n\"Best regards,\\nHR Team\"\n}}",
        "options": {}
      },
      "type": "n8n-nodes-base.gmail",
      "typeVersion": 2.2,
      "position": [
        1552,
        432
      ],
      "id": "e59fc5cb-2566-46c1-a86a-19a3176372d8",
      "name": "approve email",
      "webhookId": "b0d2ac7d-e1c9-4f3b-ad13-a0cddb7e8a90",
      "credentials": {
        "gmailOAuth2": {
          "id": "nJ2aKvguIpwbeuEN",
          "name": "Gmail account"
        }
      }
    },
    {
      "parameters": {
        "assignments": {
          "assignments": [
            {
              "id": "682e8c96-41a0-4fca-b936-b202705fd4ce",
              "name": "hr_decision",
              "value": "Approve",
              "type": "string"
            }
          ]
        },
        "includeOtherFields": true,
        "options": {}
      },
      "type": "n8n-nodes-base.set",
      "typeVersion": 3.4,
      "position": [
        1184,
        544
      ],
      "id": "08779edc-b2d2-4fd2-9c3e-02d19759ab01",
      "name": "HR Decision"
    },
    {
      "parameters": {
        "conditions": {
          "options": {
            "caseSensitive": true,
            "leftValue": "",
            "typeValidation": "strict",
            "version": 3
          },
          "conditions": [
            {
              "id": "4e83100c-0283-4de8-94ee-eb62aeea6212",
              "leftValue": "={{ $json.hr_decision }}",
              "rightValue": "Approve",
              "operator": {
                "type": "string",
                "operation": "equals",
                "name": "filter.operator.equals"
              }
            }
          ],
          "combinator": "and"
        },
        "options": {}
      },
      "type": "n8n-nodes-base.if",
      "typeVersion": 2.3,
      "position": [
        1328,
        544
      ],
      "id": "df00e8ca-2465-41ed-8f49-db2e5910c2d7",
      "name": "If"
    },
    {
      "parameters": {
        "respondWith": "json",
        "options": {}
      },
      "type": "n8n-nodes-base.respondToWebhook",
      "typeVersion": 1.5,
      "position": [
        1888,
        544
      ],
      "id": "61dd8c9c-65ec-4a0b-81f1-4dbeba9b8127",
      "name": "Respond to Webhook1"
    },
    {
      "parameters": {
        "sendTo": "isabellalomb2004@gmail.com",
        "subject": "={{ \"Candidate Ready for Interview: \" + (($json[\"Name\"] || $json.candidate_name) || \"\") }}",
        "message": "={{\n\"Hello,\\n\\n\" +\n\"The candidate \" + (($json[\"Name\"] || $json.candidate_name) || \"Applicant\") +\n\" is ready for review by HR \"\n}}",
        "options": {}
      },
      "type": "n8n-nodes-base.gmail",
      "typeVersion": 2.2,
      "position": [
        1040,
        544
      ],
      "id": "194031c4-35f2-4d31-b097-5b5b5922088e",
      "name": "Send a message",
      "webhookId": "3aaf6248-329f-42c3-b458-da28d214ea44",
      "credentials": {
        "gmailOAuth2": {
          "id": "nJ2aKvguIpwbeuEN",
          "name": "Gmail account"
        }
      }
    }
  ],
  "connections": {
    "Simple Memory": {
      "ai_memory": [
        [
          {
            "node": "AI Agent1",
            "type": "ai_memory",
            "index": 0
          }
        ]
      ]
    },
    "Append row in sheet": {
      "main": [
        [
          {
            "node": "Send a message",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Merge All Strategies1": {
      "main": [
        [
          {
            "node": "Prepare Final Output",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Generate Medium Candidate Strategy1": {
      "main": [
        [
          {
            "node": "Merge All Strategies1",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Route by Classification1": {
      "main": [
        [
          {
            "node": "Generate High Candidate Strategy",
            "type": "main",
            "index": 0
          }
        ],
        [
          {
            "node": "Generate Medium Candidate Strategy1",
            "type": "main",
            "index": 0
          }
        ],
        [
          {
            "node": "Generate High Risk Strategy",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Bias Detection Agent1": {
      "main": [
        [
          {
            "node": "Parse Agent 3 Output",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Candidate Understanding Agent1": {
      "main": [
        [
          {
            "node": "Code in JavaScript1",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Prepare Final Output": {
      "main": [
        [
          {
            "node": "Append row in sheet",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Parse Agent 3 Output": {
      "main": [
        [
          {
            "node": "AI Agent1",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "OpenAI Chat Model1": {
      "ai_languageModel": [
        [
          {
            "node": "Candidate Understanding Agent1",
            "type": "ai_languageModel",
            "index": 0
          }
        ]
      ]
    },
    "Webhook1": {
      "main": [
        [
          {
            "node": "prepare webhoook input",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Generate High Risk Strategy": {
      "main": [
        [
          {
            "node": "Merge All Strategies1",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Generate High Candidate Strategy": {
      "main": [
        [
          {
            "node": "Merge All Strategies1",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "OpenAI Chat Model6": {
      "ai_languageModel": [
        [
          {
            "node": "Bias Detection Agent1",
            "type": "ai_languageModel",
            "index": 0
          }
        ]
      ]
    },
    "Code in JavaScript3": {
      "main": [
        [
          {
            "node": "If1",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "OpenAI Chat Model (Agent 4)1": {
      "ai_languageModel": [
        [
          {
            "node": "AI Agent1",
            "type": "ai_languageModel",
            "index": 0
          }
        ]
      ]
    },
    "AI Agent1": {
      "main": [
        [
          {
            "node": "Code in JavaScript3",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "If1": {
      "main": [
        [
          {
            "node": "Route by Classification1",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "OpenAI Chat Model3": {
      "ai_languageModel": [
        [
          {
            "node": "AI Agent2",
            "type": "ai_languageModel",
            "index": 0
          }
        ]
      ]
    },
    "Parse Agent 2 Output1": {
      "main": [
        [
          {
            "node": "Bias Detection Agent1",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "AI Agent2": {
      "main": [
        [
          {
            "node": "Parse Agent 2 Output1",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Code in JavaScript1": {
      "main": [
        [
          {
            "node": "AI Agent2",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Embeddings OpenAI": {
      "ai_embedding": [
        [
          {
            "node": "Simple Vector Store1",
            "type": "ai_embedding",
            "index": 0
          }
        ]
      ]
    },
    "Simple Vector Store1": {
      "ai_tool": [
        [
          {
            "node": "AI Agent2",
            "type": "ai_tool",
            "index": 0
          }
        ]
      ]
    },
    "prepare webhoook input": {
      "main": [
        [
          {
            "node": "Candidate Understanding Agent1",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Reject email": {
      "main": [
        [
          {
            "node": "Respond to Webhook1",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "approve email": {
      "main": [
        [
          {
            "node": "Respond to Webhook1",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "HR Decision": {
      "main": [
        [
          {
            "node": "If",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "If": {
      "main": [
        [
          {
            "node": "approve email",
            "type": "main",
            "index": 0
          }
        ],
        [
          {
            "node": "Reject email",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Send a message": {
      "main": [
        [
          {
            "node": "HR Decision",
            "type": "main",
            "index": 0
          }
        ]
      ]
    }
  },
  "pinData": {},
  "meta": {
    "templateCredsSetupCompleted": true,
    "instanceId": "e2e10abc6dc85c0cf7b65433132c5fae1247a20cc7692b5561e81b4879271990"
  }
}
