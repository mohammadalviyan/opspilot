# Database Schema — OpsPilot

## Purpose

This document defines the intended MVP database entities. The actual schema must be implemented through migrations.

## Core Tables

### roles

- id
- code
- name
- created_at
- updated_at

### users

- id
- role_id
- department_id
- name
- email
- password_hash
- status
- created_at
- updated_at
- deleted_at

### departments

- id
- name
- code
- created_at
- updated_at

### applications

- id
- name
- code
- description
- owner_department_id
- created_at
- updated_at

### ticket_categories

- id
- name
- description
- created_at
- updated_at

### tickets

- id
- ticket_no
- title
- description
- type
- category_id
- priority
- status
- reporter_id
- assignee_id
- department_id
- robot_id
- application_id
- sla_due_at
- started_at
- resolved_at
- closed_at
- root_cause
- resolution
- workaround
- created_at
- updated_at
- deleted_at

### ticket_comments

- id
- ticket_id
- user_id
- message
- is_internal
- created_at

### ticket_activities

- id
- ticket_id
- actor_id
- event_type
- old_value
- new_value
- description
- created_at

### robots

- id
- name
- code
- description
- owner_department_id
- application_id
- environment
- status
- server_name
- server_ip
- schedule_info
- created_at
- updated_at
- deleted_at

### robot_run_logs

- id
- robot_id
- run_no
- status
- started_at
- finished_at
- duration_seconds
- total_transaction
- success_transaction
- failed_transaction
- error_message
- log_summary
- related_ticket_id
- created_at

### assets

- id
- name
- type
- owner
- vendor
- related_robot_id
- related_application_id
- start_date
- expired_date
- reminder_days
- status
- notes
- created_at
- updated_at
- deleted_at

### escalations

- id
- ticket_id
- target_type
- target_name
- reason
- status
- escalated_at
- resolved_at
- created_by
- created_at
- updated_at

### knowledge_base

- id
- title
- issue_pattern
- root_cause
- solution
- workaround
- related_application_id
- related_robot_id
- tags
- created_by
- created_at
- updated_at

### sla_rules

- id
- priority
- target_minutes
- is_active
- created_at
- updated_at

## Index Recommendations

- tickets.ticket_no unique
- tickets.status
- tickets.priority
- tickets.type
- tickets.assignee_id
- tickets.reporter_id
- tickets.robot_id
- tickets.application_id
- tickets.created_at
- ticket_comments.ticket_id
- ticket_activities.ticket_id
- robots.code unique
- robot_run_logs.robot_id
- robot_run_logs.started_at
- assets.expired_date
- assets.status

## Migration Guardrail

Every schema change must be added as a migration file. Do not modify a previously applied migration once shared.
