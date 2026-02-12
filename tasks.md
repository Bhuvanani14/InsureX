# Implementation Plan: InsureX

## Overview

This implementation plan breaks down the InsureX platform into discrete, manageable coding tasks. The approach follows an incremental development strategy, starting with core infrastructure and data models, then building out individual services, and finally integrating everything into a cohesive platform.

Given the scope of InsureX, this plan focuses on building a functional hackathon prototype that demonstrates the core value proposition: AI-powered insurance understanding with transparent, grounded recommendations.

## Tasks

- [ ] 1. Project setup and core infrastructure
  - Set up TypeScript project with Node.js backend and React frontend
  - Configure build tools (Webpack/Vite), linting (ESLint), and formatting (Prettier)
  - Set up testing framework (Jest) and property-based testing library (fast-check)
  - Configure PostgreSQL database and Redis cache
  - Set up environment configuration for API keys and secrets
  - _Requirements: All (foundational)_

- [ ] 2. Database schema and data models
  - [ ] 2.1 Create database schema for core entities
    - Define tables for User, Policy, PurchasedPolicy, NeedProfile, Conversation
    - Define tables for VerifiedAdvisor, OnboardingSession, AuditLog
    - Set up foreign key relationships and indexes
    - Implement database migration scripts
    - _Requirements: 1.4, 9.1, 10.2, 11.5, 14.3_
  
  - [ ] 2.2 Implement TypeScript data models and validation
    - Create TypeScript interfaces matching database schema
    - Implement validation functions for all data models
    - Add data encryption utilities for sensitive fields
    - _Requirements: 11.1, 13.4_
  
  - [ ]* 2.3 Write property test for data model validation
    - **Property 2: Need Profile Generation**
    - **Validates: Requirements 1.4**
  
  - [ ]* 2.4 Write property test for data encryption
    - **Property 27: Data Encryption at Rest**
    - **Validates: Requirements 11.1**

- [ ] 3. API Gateway and authentication
  - [ ] 3.1 Implement API Gateway with Express
    - Set up Express server with middleware
    - Implement request validation and error handling
    - Add rate limiting and security headers
    - Configure CORS and request logging
    - _Requirements: 13.4_
  
  - [ ] 3.2 Implement authentication and authorization
    - Set up JWT-based authentication
    - Implement user registration and login endpoints
    - Add session management with Redis
    - Implement role-based access control
    - _Requirements: 11.2, 11.5_
  
  - [ ]* 3.3 Write unit tests for authentication flows
    - Test registration, login, and token refresh
    - Test authorization for protected endpoints
    - _Requirements: 11.2_

- [ ] 4. Policy Brochure management and processing
  - [ ] 4.1 Implement policy brochure upload and storage
    - Create endpoint for brochure upload
    - Implement file validation (format, size)
    - Store brochures in S3 or local storage
    - Create database records for uploaded brochures
    - _Requirements: 14.1, 14.4_
  
  - [ ] 4.2 Implement policy brochure parsing and extraction
    - Parse PDF brochures using pdf-parse or similar
    - Extract structured data (coverage, exclusions, terms)
    - Store extracted data in database
    - Handle parsing errors gracefully
    - _Requirements: 14.2_
  
  - [ ] 4.3 Implement brochure versioning system
    - Create version tracking for brochure updates
    - Maintain history of all brochure versions
    - Link recommendations to specific brochure versions
    - _Requirements: 14.3_
  
  - [ ]* 4.4 Write property test for brochure validation
    - **Property 31: Policy Brochure Validation**
    - **Validates: Requirements 14.1**
  
  - [ ]* 4.5 Write property test for data extraction completeness
    - **Property 6: Policy Data Extraction Completeness**
    - **Validates: Requirements 3.1, 14.2**
  
  - [ ]* 4.6 Write property test for brochure versioning
    - **Property 32: Policy Brochure Versioning**
    - **Validates: Requirements 14.3**

- [ ] 5. Vector database and RAG pipeline
  - [ ] 5.1 Set up vector database (Pinecone or in-memory)
    - Initialize vector database connection
    - Create indexes for policy brochure embeddings
    - Implement embedding generation using OpenAI or Sentence Transformers
    - _Requirements: 2.1_
  
  - [ ] 5.2 Implement RAG (Retrieval-Augmented Generation) pipeline
    - Create function to generate embeddings for queries
    - Implement similarity search in vector database
    - Retrieve top-k relevant policy sections
    - Format retrieved sections for LLM context
    - _Requirements: 2.1, 2.3_
  
  - [ ]* 5.3 Write unit tests for RAG pipeline
    - Test embedding generation
    - Test similarity search with known queries
    - Test context formatting
    - _Requirements: 2.1_

- [ ] 6. AI Advisor Service
  - [ ] 6.1 Implement AI Advisor core functionality
    - Create service class for AI Advisor
    - Implement query processing with LLM (OpenAI GPT-4)
    - Integrate RAG pipeline for grounded responses
    - Add conversation context management
    - _Requirements: 2.1, 2.3_
  
  - [ ] 6.2 Implement source citation extraction
    - Parse LLM responses for factual claims
    - Match claims to source brochure sections
    - Generate SourceCitation objects with references
    - _Requirements: 2.3, 8.2_
  
  - [ ] 6.3 Implement out-of-scope query detection
    - Detect when queries cannot be answered from brochures
    - Return appropriate "information not available" responses
    - _Requirements: 2.4_
  
  - [ ]* 6.4 Write property test for response grounding
    - **Property 3: Response Grounding in Source Documents**
    - **Validates: Requirements 2.1**
  
  - [ ]* 6.5 Write property test for source citation completeness
    - **Property 4: Source Citation Completeness**
    - **Validates: Requirements 2.3, 8.2, 8.5**
  
  - [ ]* 6.6 Write property test for out-of-scope handling
    - **Property 5: Out-of-Scope Query Handling**
    - **Validates: Requirements 2.4**

- [ ] 7. Checkpoint - Ensure core AI functionality works
  - Test AI Advisor with sample policy brochures
  - Verify grounding and citation generation
  - Ensure all tests pass
  - Ask the user if questions arise

- [ ] 8. Onboarding Service
  - [ ] 8.1 Implement onboarding flow logic
    - Create OnboardingService class
    - Define question flow based on life-stage
    - Implement adaptive questioning logic
    - Store onboarding session state in Redis
    - _Requirements: 1.1, 1.3_
  
  - [ ] 8.2 Implement Need Profile generation
    - Collect and validate onboarding answers
    - Generate NeedProfile from collected data
    - Store NeedProfile in database
    - _Requirements: 1.4_
  
  - [ ] 8.3 Create onboarding API endpoints
    - POST /api/onboarding/start
    - POST /api/onboarding/answer
    - POST /api/onboarding/complete
    - GET /api/onboarding/progress
    - _Requirements: 1.1, 1.3, 1.4_
  
  - [ ]* 8.4 Write property test for question adaptation
    - **Property 1: Onboarding Question Adaptation**
    - **Validates: Requirements 1.3**
  
  - [ ]* 8.5 Write property test for Need Profile generation
    - **Property 2: Need Profile Generation**
    - **Validates: Requirements 1.4**

- [ ] 9. Policy Simplification Engine
  - [ ] 9.1 Implement policy simplification logic
    - Create PolicySimplificationEngine class
    - Use LLM to convert complex language to simple terms
    - Generate examples and scenarios for coverage items
    - Identify and mark critical exclusions
    - _Requirements: 3.2, 3.3, 3.4_
  
  - [ ] 9.2 Create policy simplification API endpoints
    - GET /api/policies/:id/simplified
    - GET /api/policies/:id/terms
    - GET /api/policies/:id/examples
    - _Requirements: 3.1, 3.3_
  
  - [ ]* 9.3 Write property test for coverage examples
    - **Property 7: Coverage Examples Generation**
    - **Validates: Requirements 3.3**
  
  - [ ]* 9.4 Write property test for critical exclusion identification
    - **Property 8: Critical Exclusion Identification**
    - **Validates: Requirements 3.4**

- [ ] 10. Recommendation Service
  - [ ] 10.1 Implement recommendation matching algorithm
    - Create RecommendationService class
    - Implement policy matching based on NeedProfile
    - Calculate suitability scores for each policy
    - Filter policies by budget and priorities
    - _Requirements: 4.1_
  
  - [ ] 10.2 Implement recommendation explanation generation
    - Generate "why recommended" explanations using LLM
    - Generate "why not recommended" explanations
    - Identify matched and unmatched criteria
    - _Requirements: 4.2, 4.3, 8.1_
  
  - [ ] 10.3 Implement recommendation ranking and updates
    - Sort recommendations by suitability score
    - Implement preference update logic
    - Recalculate recommendations on preference changes
    - _Requirements: 4.4, 4.5_
  
  - [ ] 10.4 Create recommendation API endpoints
    - GET /api/recommendations
    - PUT /api/recommendations/preferences
    - GET /api/recommendations/:policyId/reasoning
    - _Requirements: 4.1, 4.2, 4.3, 4.4, 4.5_
  
  - [ ]* 10.5 Write property test for recommendation matching
    - **Property 9: Recommendation Matching**
    - **Validates: Requirements 4.1**
  
  - [ ]* 10.6 Write property test for explanation completeness
    - **Property 10: Recommendation Explanation Completeness**
    - **Validates: Requirements 4.2, 4.3, 8.1**
  
  - [ ]* 10.7 Write property test for recommendation updates
    - **Property 11: Recommendation Update on Preference Change**
    - **Validates: Requirements 4.4**
  
  - [ ]* 10.8 Write property test for ranking order
    - **Property 12: Recommendation Ranking Order**
    - **Validates: Requirements 4.5**

- [ ] 11. Checkpoint - Ensure recommendation engine works
  - Test recommendations with various user profiles
  - Verify suitability scoring and ranking
  - Ensure all tests pass
  - Ask the user if questions arise

- [ ] 12. Comparison Service
  - [ ] 12.1 Implement policy comparison logic
    - Create ComparisonService class
    - Extract comparison data from multiple policies
    - Generate comparison matrix with all key aspects
    - Calculate visual indicators (ratings) for each value
    - _Requirements: 5.2, 5.3_
  
  - [ ] 12.2 Implement trade-off summary generation
    - Use LLM to generate trade-off summaries
    - Identify key differences between policies
    - Determine "best for" scenarios
    - _Requirements: 5.4_
  
  - [ ] 12.3 Implement comparison policy limit
    - Validate comparison requests (max 4 policies)
    - Return error for requests exceeding limit
    - _Requirements: 5.5_
  
  - [ ] 12.4 Create comparison API endpoints
    - POST /api/compare (with policy IDs in body)
    - GET /api/compare/:comparisonId/tradeoffs
    - _Requirements: 5.1, 5.2, 5.3, 5.4, 5.5_
  
  - [ ]* 12.5 Write property test for comparison data completeness
    - **Property 13: Comparison Data Completeness**
    - **Validates: Requirements 5.2**
  
  - [ ]* 12.6 Write property test for visual indicators
    - **Property 14: Comparison Visual Indicators**
    - **Validates: Requirements 5.3**
  
  - [ ]* 12.7 Write property test for trade-off summary
    - **Property 15: Trade-off Summary Generation**
    - **Validates: Requirements 5.4**
  
  - [ ]* 12.8 Write property test for policy limit
    - **Property 16: Comparison Policy Limit**
    - **Validates: Requirements 5.5**

- [ ] 13. Scenario Simulator
  - [ ] 13.1 Implement scenario simulation logic
    - Create ScenarioSimulator class
    - Define preset scenarios (hospitalization, accident, etc.)
    - Implement custom scenario creation
    - Calculate coverage outcomes for each policy
    - _Requirements: 6.1, 6.4_
  
  - [ ] 13.2 Implement cost breakdown calculation
    - Calculate covered amounts based on policy terms
    - Identify exclusions for each scenario
    - Calculate out-of-pocket costs
    - Generate itemized cost breakdowns
    - _Requirements: 6.3_
  
  - [ ] 13.3 Implement scenario comparison
    - Compare outcomes across multiple policies
    - Highlight differences in coverage
    - Generate comparison summaries
    - _Requirements: 6.5_
  
  - [ ] 13.4 Create scenario simulator API endpoints
    - POST /api/scenarios/simulate
    - POST /api/scenarios/custom
    - GET /api/scenarios/presets
    - POST /api/scenarios/compare
    - _Requirements: 6.1, 6.2, 6.3, 6.4, 6.5_
  
  - [ ]* 13.5 Write property test for simulation result generation
    - **Property 17: Scenario Simulation Result Generation**
    - **Validates: Requirements 6.1, 6.2**
  
  - [ ]* 13.6 Write property test for outcome completeness
    - **Property 18: Simulation Outcome Completeness**
    - **Validates: Requirements 6.3**
  
  - [ ]* 13.7 Write property test for coverage difference highlighting
    - **Property 19: Scenario Coverage Difference Highlighting**
    - **Validates: Requirements 6.5**

- [ ] 14. Multilingual support
  - [ ] 14.1 Implement translation service integration
    - Integrate with translation API (Google Translate or similar)
    - Create translation caching layer
    - Implement language detection
    - _Requirements: 7.1, 7.2_
  
  - [ ] 14.2 Implement language selection and content translation
    - Add language parameter to all API endpoints
    - Translate AI responses based on selected language
    - Translate UI elements and static content
    - _Requirements: 7.2_
  
  - [ ]* 14.3 Write property test for translation consistency
    - **Property 20: Language Translation Consistency**
    - **Validates: Requirements 7.2**

- [ ] 15. Trust and transparency features
  - [ ] 15.1 Implement pricing transparency
    - Display all cost components in pricing views
    - Itemize premiums, fees, and charges
    - Ensure no hidden costs in any display
    - _Requirements: 8.3_
  
  - [ ]* 15.2 Write property test for pricing transparency
    - **Property 21: Pricing Transparency**
    - **Validates: Requirements 8.3**

- [ ] 16. Verified Advisor management
  - [ ] 16.1 Implement advisor profile management
    - Create VerifiedAdvisor data model
    - Implement advisor verification workflow
    - Store advisor credentials and details
    - _Requirements: 9.1, 9.3_
  
  - [ ] 16.2 Implement advisor connection and consent
    - Create advisor browsing and search
    - Implement connection request functionality
    - Add explicit consent flow for data sharing
    - Record consent in audit logs
    - _Requirements: 9.2, 9.4_
  
  - [ ] 16.3 Create advisor API endpoints
    - GET /api/advisors
    - GET /api/advisors/:id
    - POST /api/advisors/connect
    - POST /api/advisors/:id/review
    - _Requirements: 9.1, 9.2, 9.5_
  
  - [ ]* 16.4 Write property test for advisor profile completeness
    - **Property 22: Advisor Profile Completeness**
    - **Validates: Requirements 9.1**
  
  - [ ]* 16.5 Write property test for advisor verification
    - **Property 23: Advisor Verification Requirement**
    - **Validates: Requirements 9.3**
  
  - [ ]* 16.6 Write property test for data sharing consent
    - **Property 24: Data Sharing Consent**
    - **Validates: Requirements 9.4, 11.2**

- [ ] 17. Checkpoint - Ensure all backend services are integrated
  - Test end-to-end flows from onboarding to comparison
  - Verify all API endpoints work correctly
  - Ensure all tests pass
  - Ask the user if questions arise

- [ ] 18. Policy purchase and management
  - [ ] 18.1 Implement policy purchase flow
    - Create purchase initiation endpoint
    - Integrate with mock payment gateway
    - Create PurchasedPolicy records on success
    - _Requirements: 10.1_
  
  - [ ] 18.2 Implement Policy Locker
    - Store purchased policies in user's locker
    - Implement policy retrieval and display
    - Add policy document storage
    - _Requirements: 10.2, 10.4_
  
  - [ ] 18.3 Implement renewal reminders
    - Create scheduled job for renewal checks
    - Send reminders before policy expiration
    - Track reminder delivery status
    - _Requirements: 10.3_
  
  - [ ]* 18.4 Write property test for policy locker storage
    - **Property 25: Policy Locker Storage and Retrieval**
    - **Validates: Requirements 10.2, 10.4**
  
  - [ ]* 18.5 Write property test for renewal reminders
    - **Property 26: Renewal Reminder Scheduling**
    - **Validates: Requirements 10.3**

- [ ] 19. Data security and privacy
  - [ ] 19.1 Implement data deletion functionality
    - Create data deletion endpoint
    - Implement cascading deletion of user data
    - Schedule deletion within 30-day window
    - _Requirements: 11.4_
  
  - [ ] 19.2 Implement audit logging
    - Create audit log entries for all data operations
    - Log access, modifications, and deletions
    - Store logs securely with tamper protection
    - _Requirements: 11.5_
  
  - [ ]* 19.3 Write property test for data deletion
    - **Property 28: Data Deletion Compliance**
    - **Validates: Requirements 11.4**
  
  - [ ]* 19.4 Write property test for audit log creation
    - **Property 29: Audit Log Creation**
    - **Validates: Requirements 11.5**

- [ ] 20. Error handling and user feedback
  - [ ] 20.1 Implement comprehensive error handling
    - Create standardized error response format
    - Add error handling middleware to API Gateway
    - Implement specific error handlers for each service
    - Generate clear, actionable error messages
    - _Requirements: 13.4_
  
  - [ ]* 20.2 Write property test for error message generation
    - **Property 30: Error Message Generation**
    - **Validates: Requirements 13.4**

- [ ] 21. Loyalty program
  - [ ] 21.1 Implement loyalty points system
    - Award points on purchase completion
    - Track loyalty points in user profile
    - Implement tier calculation logic
    - _Requirements: 15.1_
  
  - [ ] 21.2 Implement renewal incentives
    - Generate incentives for policy renewals
    - Apply discounts or bonuses for renewals
    - _Requirements: 15.2_
  
  - [ ] 21.3 Implement loyalty notifications
    - Send notifications when points are earned
    - Send notifications on tier changes
    - Notify users of unlocked benefits
    - _Requirements: 15.4, 15.5_
  
  - [ ]* 21.4 Write property test for loyalty points award
    - **Property 34: Loyalty Points Award**
    - **Validates: Requirements 15.1**
  
  - [ ]* 21.5 Write property test for renewal incentives
    - **Property 35: Renewal Incentive Provision**
    - **Validates: Requirements 15.2**
  
  - [ ]* 21.6 Write property test for loyalty notifications
    - **Property 36: Loyalty Notification**
    - **Validates: Requirements 15.4, 15.5**

- [ ] 22. Frontend - Core UI components
  - [ ] 22.1 Create reusable UI component library
    - Build Button, Input, Card, Modal components
    - Create PolicyCard component for displaying policies
    - Create ComparisonTable component
    - Implement responsive design with TailwindCSS
    - _Requirements: 13.1, 13.2_
  
  - [ ] 22.2 Implement authentication UI
    - Create login and registration pages
    - Add password reset flow
    - Implement protected route wrapper
    - _Requirements: 11.2_

- [ ] 23. Frontend - Onboarding flow
  - [ ] 23.1 Create onboarding UI
    - Build step-by-step onboarding wizard
    - Implement question rendering based on type
    - Add progress indicator
    - Handle form validation and submission
    - _Requirements: 1.1, 1.3_
  
  - [ ] 23.2 Integrate onboarding with backend
    - Connect to onboarding API endpoints
    - Handle session management
    - Display generated Need Profile
    - _Requirements: 1.4_

- [ ] 24. Frontend - AI Advisor chat interface
  - [ ] 24.1 Create chat UI
    - Build chat message list component
    - Create message input with send button
    - Display source citations in messages
    - Add loading states for AI responses
    - _Requirements: 2.1, 2.3_
  
  - [ ] 24.2 Integrate chat with AI Advisor backend
    - Connect to AI Advisor API
    - Handle conversation context
    - Display follow-up suggestions
    - _Requirements: 2.1, 2.3, 2.4_

- [ ] 25. Frontend - Policy recommendations and comparison
  - [ ] 25.1 Create recommendations page
    - Display recommended policies with suitability scores
    - Show "why recommended" explanations
    - Add filters for budget and preferences
    - Implement policy selection for comparison
    - _Requirements: 4.1, 4.2, 4.5_
  
  - [ ] 25.2 Create comparison page
    - Build side-by-side comparison view
    - Display comparison matrix with visual indicators
    - Show trade-off summary
    - Add "select policy" action buttons
    - _Requirements: 5.1, 5.2, 5.3, 5.4_

- [ ] 26. Frontend - Scenario simulator
  - [ ] 26.1 Create scenario simulator UI
    - Build scenario selection interface
    - Create custom scenario form
    - Display simulation results with cost breakdowns
    - Show coverage comparison across policies
    - _Requirements: 6.1, 6.3, 6.4, 6.5_
  
  - [ ] 26.2 Integrate simulator with backend
    - Connect to scenario simulator API
    - Handle preset and custom scenarios
    - Display results in user-friendly format
    - _Requirements: 6.1, 6.2, 6.3_

- [ ] 27. Frontend - Policy purchase and locker
  - [ ] 27.1 Create purchase flow UI
    - Build policy purchase page with pricing details
    - Integrate mock payment form
    - Show purchase confirmation
    - _Requirements: 10.1_
  
  - [ ] 27.2 Create Policy Locker UI
    - Display all purchased policies
    - Show policy details and documents
    - Add renewal reminder indicators
    - _Requirements: 10.2, 10.4_

- [ ] 28. Frontend - Advisor marketplace
  - [ ] 28.1 Create advisor browsing UI
    - Display advisor profile cards
    - Add search and filter functionality
    - Show advisor ratings and reviews
    - _Requirements: 9.1_
  
  - [ ] 28.2 Create advisor connection flow
    - Build connection request modal
    - Implement consent checkbox
    - Show connection status
    - Add review submission form
    - _Requirements: 9.2, 9.4, 9.5_

- [ ] 29. Frontend - User profile and settings
  - [ ] 29.1 Create user profile page
    - Display user information and Need Profile
    - Show loyalty points and tier
    - Add language selection
    - _Requirements: 7.1, 15.3_
  
  - [ ] 29.2 Create settings page
    - Add data deletion request option
    - Show consent management
    - Display notification preferences
    - _Requirements: 11.4_

- [ ] 30. Integration and end-to-end testing
  - [ ] 30.1 Implement integration tests
    - Test complete onboarding to recommendation flow
    - Test policy comparison with real data
    - Test scenario simulation end-to-end
    - Test purchase flow with mock payment
    - _Requirements: All_
  
  - [ ]* 30.2 Run all property-based tests
    - Execute all 36 property tests with 100+ iterations
    - Verify all properties pass
    - Fix any failures discovered
    - _Requirements: All testable requirements_

- [ ] 31. Final checkpoint and demo preparation
  - Ensure all core features work end-to-end
  - Test with sample policy brochures
  - Verify AI responses are grounded and accurate
  - Prepare demo script and sample data
  - Ask the user if questions arise

## Notes

- Tasks marked with `*` are optional and can be skipped for faster MVP development
- Each task references specific requirements for traceability
- Checkpoints ensure incremental validation throughout development
- Property tests validate universal correctness properties
- Unit tests validate specific examples and edge cases
- The implementation follows a backend-first approach, then frontend integration
- For hackathon prototype, focus on tasks 1-17 and 22-26 for core functionality
