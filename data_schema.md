# Gemini CLI Telemetry — Data Schemas (All Events)

本文件列出 Gemini CLI 所有日志事件的 JSON
Schema（每个事件一个 schema），用于下游数据处理平台接收与校验。所有事件都包含通用字段
`common`。

---

## Common Attributes (all events)

```json
{
  "common": {
    "type": "object",
    "required": ["session.id", "installation.id", "interactive"],
    "properties": {
      "session.id": { "type": "string" },
      "installation.id": { "type": "string" },
      "interactive": { "type": "boolean" },
      "user.email": { "type": "string", "format": "email" }
    }
  }
}
```

---

## 1. Sessions

### gemini_cli.config

```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "gemini_cli.config",
  "type": "object",
  "required": ["event.name", "event.timestamp", "common"],
  "properties": {
    "event.name": { "const": "config" },
    "event.timestamp": { "type": "string", "format": "date-time" },
    "common": { "$ref": "#/definitions/common" },
    "model": { "type": "string" },
    "embedding_model": { "type": "string" },
    "sandbox_enabled": { "type": "boolean" },
    "core_tools_enabled": { "type": "string" },
    "approval_mode": { "type": "string" },
    "api_key_enabled": { "type": "boolean" },
    "vertex_ai_enabled": { "type": "boolean" },
    "log_user_prompts_enabled": { "type": "boolean" },
    "file_filtering_respect_git_ignore": { "type": "boolean" },
    "debug_mode": { "type": "boolean" },
    "mcp_servers": { "type": "string" },
    "mcp_servers_count": { "type": "integer" },
    "extensions": { "type": "string" },
    "extension_ids": { "type": "string" },
    "extension_count": { "type": "integer" },
    "mcp_tools": { "type": "string" },
    "mcp_tools_count": { "type": "integer" },
    "output_format": { "type": "string" }
  },
  "definitions": { "common": { "$ref": "#/common" } }
}
```

### gemini_cli.user_prompt

```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "gemini_cli.user_prompt",
  "type": "object",
  "required": [
    "event.name",
    "event.timestamp",
    "common",
    "prompt_length",
    "prompt_id"
  ],
  "properties": {
    "event.name": { "const": "user_prompt" },
    "event.timestamp": { "type": "string", "format": "date-time" },
    "common": { "$ref": "#/definitions/common" },
    "prompt_length": { "type": "integer", "minimum": 0 },
    "prompt_id": { "type": "string" },
    "prompt": { "type": "string" },
    "auth_type": { "type": "string" }
  },
  "definitions": { "common": { "$ref": "#/common" } }
}
```

---

## 2. Tools

### gemini_cli.tool_call

```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "gemini_cli.tool_call",
  "type": "object",
  "required": [
    "event.name",
    "event.timestamp",
    "common",
    "function_name",
    "function_args",
    "duration_ms",
    "success",
    "prompt_id",
    "tool_type"
  ],
  "properties": {
    "event.name": { "const": "tool_call" },
    "event.timestamp": { "type": "string", "format": "date-time" },
    "common": { "$ref": "#/definitions/common" },
    "function_name": { "type": "string" },
    "function_args": { "type": "object" },
    "duration_ms": { "type": "integer", "minimum": 0 },
    "success": { "type": "boolean" },
    "decision": { "type": "string" },
    "error": { "type": "string" },
    "error_type": { "type": "string" },
    "prompt_id": { "type": "string" },
    "tool_type": { "type": "string" },
    "mcp_server_name": { "type": "string" },
    "extension_name": { "type": "string" },
    "extension_id": { "type": "string" },
    "content_length": { "type": "integer" },
    "metadata": { "type": "object" }
  },
  "definitions": { "common": { "$ref": "#/common" } }
}
```

### gemini_cli.tool_output_truncated

```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "gemini_cli.tool_output_truncated",
  "type": "object",
  "required": [
    "event.name",
    "event.timestamp",
    "common",
    "tool_name",
    "prompt_id"
  ],
  "properties": {
    "event.name": { "const": "tool_output_truncated" },
    "event.timestamp": { "type": "string", "format": "date-time" },
    "common": { "$ref": "#/definitions/common" },
    "tool_name": { "type": "string" },
    "original_content_length": { "type": "integer" },
    "truncated_content_length": { "type": "integer" },
    "threshold": { "type": "integer" },
    "lines": { "type": "integer" },
    "prompt_id": { "type": "string" }
  },
  "definitions": { "common": { "$ref": "#/common" } }
}
```

### gemini_cli.edit_strategy

```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "gemini_cli.edit_strategy",
  "type": "object",
  "required": ["event.name", "event.timestamp", "common", "strategy"],
  "properties": {
    "event.name": { "const": "edit_strategy" },
    "event.timestamp": { "type": "string", "format": "date-time" },
    "common": { "$ref": "#/definitions/common" },
    "strategy": { "type": "string" }
  },
  "definitions": { "common": { "$ref": "#/common" } }
}
```

### gemini_cli.edit_correction

```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "gemini_cli.edit_correction",
  "type": "object",
  "required": ["event.name", "event.timestamp", "common", "correction"],
  "properties": {
    "event.name": { "const": "edit_correction" },
    "event.timestamp": { "type": "string", "format": "date-time" },
    "common": { "$ref": "#/definitions/common" },
    "correction": { "type": "string", "enum": ["success", "failure"] }
  },
  "definitions": { "common": { "$ref": "#/common" } }
}
```

---

## 3. Files

### gemini_cli.file_operation

```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "gemini_cli.file_operation",
  "type": "object",
  "required": [
    "event.name",
    "event.timestamp",
    "common",
    "tool_name",
    "operation"
  ],
  "properties": {
    "event.name": { "const": "file_operation" },
    "event.timestamp": { "type": "string", "format": "date-time" },
    "common": { "$ref": "#/definitions/common" },
    "tool_name": { "type": "string" },
    "operation": { "type": "string", "enum": ["create", "read", "update"] },
    "lines": { "type": "integer" },
    "mimetype": { "type": "string" },
    "extension": { "type": "string" },
    "programming_language": { "type": "string" }
  },
  "definitions": { "common": { "$ref": "#/common" } }
}
```

---

## 4. API

### gemini_cli.api_request

```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "gemini_cli.api_request",
  "type": "object",
  "required": ["event.name", "event.timestamp", "common", "model", "prompt_id"],
  "properties": {
    "event.name": { "const": "api_request" },
    "event.timestamp": { "type": "string", "format": "date-time" },
    "common": { "$ref": "#/definitions/common" },
    "model": { "type": "string" },
    "prompt_id": { "type": "string" },
    "request_text": { "type": "string" }
  },
  "definitions": { "common": { "$ref": "#/common" } }
}
```

### gemini_cli.api_response

```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "gemini_cli.api_response",
  "type": "object",
  "required": [
    "event.name",
    "event.timestamp",
    "common",
    "model",
    "duration_ms",
    "prompt_id"
  ],
  "properties": {
    "event.name": { "const": "api_response" },
    "event.timestamp": { "type": "string", "format": "date-time" },
    "common": { "$ref": "#/definitions/common" },
    "model": { "type": "string" },
    "status_code": { "type": ["integer", "string"] },
    "duration_ms": { "type": "integer" },
    "input_token_count": { "type": "integer" },
    "output_token_count": { "type": "integer" },
    "cached_content_token_count": { "type": "integer" },
    "thoughts_token_count": { "type": "integer" },
    "tool_token_count": { "type": "integer" },
    "total_token_count": { "type": "integer" },
    "response_text": { "type": "string" },
    "prompt_id": { "type": "string" },
    "auth_type": { "type": "string" },
    "finish_reasons": { "type": "array", "items": { "type": "string" } }
  },
  "definitions": { "common": { "$ref": "#/common" } }
}
```

### gemini_cli.api_error

```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "gemini_cli.api_error",
  "type": "object",
  "required": [
    "event.name",
    "event.timestamp",
    "common",
    "model",
    "error",
    "duration_ms",
    "prompt_id"
  ],
  "properties": {
    "event.name": { "const": "api_error" },
    "event.timestamp": { "type": "string", "format": "date-time" },
    "common": { "$ref": "#/definitions/common" },
    "model": { "type": "string" },
    "error": { "type": "string" },
    "error_type": { "type": "string" },
    "status_code": { "type": ["integer", "string"] },
    "duration_ms": { "type": "integer" },
    "prompt_id": { "type": "string" },
    "auth_type": { "type": "string" }
  },
  "definitions": { "common": { "$ref": "#/common" } }
}
```

### gemini_cli.malformed_json_response

```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "gemini_cli.malformed_json_response",
  "type": "object",
  "required": ["event.name", "event.timestamp", "common", "model"],
  "properties": {
    "event.name": { "const": "malformed_json_response" },
    "event.timestamp": { "type": "string", "format": "date-time" },
    "common": { "$ref": "#/definitions/common" },
    "model": { "type": "string" }
  },
  "definitions": { "common": { "$ref": "#/common" } }
}
```

---

## 5. Model routing

### gemini_cli.slash_command

```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "gemini_cli.slash_command",
  "type": "object",
  "required": ["event.name", "event.timestamp", "common", "command", "status"],
  "properties": {
    "event.name": { "const": "slash_command" },
    "event.timestamp": { "type": "string", "format": "date-time" },
    "common": { "$ref": "#/definitions/common" },
    "command": { "type": "string" },
    "subcommand": { "type": "string" },
    "status": { "type": "string", "enum": ["success", "error"] }
  },
  "definitions": { "common": { "$ref": "#/common" } }
}
```

### gemini_cli.slash_command.model

```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "gemini_cli.slash_command.model",
  "type": "object",
  "required": ["event.name", "event.timestamp", "common", "model_name"],
  "properties": {
    "event.name": { "const": "slash_command.model" },
    "event.timestamp": { "type": "string", "format": "date-time" },
    "common": { "$ref": "#/definitions/common" },
    "model_name": { "type": "string" }
  },
  "definitions": { "common": { "$ref": "#/common" } }
}
```

### gemini_cli.model_routing

```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "gemini_cli.model_routing",
  "type": "object",
  "required": [
    "event.name",
    "event.timestamp",
    "common",
    "decision_model",
    "decision_source",
    "routing_latency_ms",
    "failed"
  ],
  "properties": {
    "event.name": { "const": "model_routing" },
    "event.timestamp": { "type": "string", "format": "date-time" },
    "common": { "$ref": "#/definitions/common" },
    "decision_model": { "type": "string" },
    "decision_source": { "type": "string" },
    "routing_latency_ms": { "type": "integer" },
    "reasoning": { "type": "string" },
    "failed": { "type": "boolean" },
    "error_message": { "type": "string" }
  },
  "definitions": { "common": { "$ref": "#/common" } }
}
```

---

## 6. Chat & streaming

### gemini_cli.chat_compression

```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "gemini_cli.chat_compression",
  "type": "object",
  "required": [
    "event.name",
    "event.timestamp",
    "common",
    "tokens_before",
    "tokens_after"
  ],
  "properties": {
    "event.name": { "const": "chat_compression" },
    "event.timestamp": { "type": "string", "format": "date-time" },
    "common": { "$ref": "#/definitions/common" },
    "tokens_before": { "type": "integer" },
    "tokens_after": { "type": "integer" }
  },
  "definitions": { "common": { "$ref": "#/common" } }
}
```

### gemini_cli.chat.invalid_chunk

```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "gemini_cli.chat.invalid_chunk",
  "type": "object",
  "required": ["event.name", "event.timestamp", "common"],
  "properties": {
    "event.name": { "const": "chat.invalid_chunk" },
    "event.timestamp": { "type": "string", "format": "date-time" },
    "common": { "$ref": "#/definitions/common" },
    "error.message": { "type": "string" }
  },
  "definitions": { "common": { "$ref": "#/common" } }
}
```

### gemini_cli.chat.content_retry

```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "gemini_cli.chat.content_retry",
  "type": "object",
  "required": [
    "event.name",
    "event.timestamp",
    "common",
    "attempt_number",
    "error_type",
    "retry_delay_ms",
    "model"
  ],
  "properties": {
    "event.name": { "const": "chat.content_retry" },
    "event.timestamp": { "type": "string", "format": "date-time" },
    "common": { "$ref": "#/definitions/common" },
    "attempt_number": { "type": "integer" },
    "error_type": { "type": "string" },
    "retry_delay_ms": { "type": "integer" },
    "model": { "type": "string" }
  },
  "definitions": { "common": { "$ref": "#/common" } }
}
```

### gemini_cli.chat.content_retry_failure

```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "gemini_cli.chat.content_retry_failure",
  "type": "object",
  "required": [
    "event.name",
    "event.timestamp",
    "common",
    "total_attempts",
    "final_error_type",
    "model"
  ],
  "properties": {
    "event.name": { "const": "chat.content_retry_failure" },
    "event.timestamp": { "type": "string", "format": "date-time" },
    "common": { "$ref": "#/definitions/common" },
    "total_attempts": { "type": "integer" },
    "final_error_type": { "type": "string" },
    "total_duration_ms": { "type": "integer" },
    "model": { "type": "string" }
  },
  "definitions": { "common": { "$ref": "#/common" } }
}
```

### gemini_cli.conversation_finished

```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "gemini_cli.conversation_finished",
  "type": "object",
  "required": [
    "event.name",
    "event.timestamp",
    "common",
    "approvalMode",
    "turnCount"
  ],
  "properties": {
    "event.name": { "const": "conversation_finished" },
    "event.timestamp": { "type": "string", "format": "date-time" },
    "common": { "$ref": "#/definitions/common" },
    "approvalMode": { "type": "string" },
    "turnCount": { "type": "integer" }
  },
  "definitions": { "common": { "$ref": "#/common" } }
}
```

### gemini_cli.next_speaker_check

```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "gemini_cli.next_speaker_check",
  "type": "object",
  "required": [
    "event.name",
    "event.timestamp",
    "common",
    "prompt_id",
    "finish_reason",
    "result"
  ],
  "properties": {
    "event.name": { "const": "next_speaker_check" },
    "event.timestamp": { "type": "string", "format": "date-time" },
    "common": { "$ref": "#/definitions/common" },
    "prompt_id": { "type": "string" },
    "finish_reason": { "type": "string" },
    "result": { "type": "string" }
  },
  "definitions": { "common": { "$ref": "#/common" } }
}
```

---

## 7. Resilience

### gemini_cli.flash_fallback

```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "gemini_cli.flash_fallback",
  "type": "object",
  "required": ["event.name", "event.timestamp", "common", "auth_type"],
  "properties": {
    "event.name": { "const": "flash_fallback" },
    "event.timestamp": { "type": "string", "format": "date-time" },
    "common": { "$ref": "#/definitions/common" },
    "auth_type": { "type": "string" }
  },
  "definitions": { "common": { "$ref": "#/common" } }
}
```

### gemini_cli.ripgrep_fallback

```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "gemini_cli.ripgrep_fallback",
  "type": "object",
  "required": ["event.name", "event.timestamp", "common"],
  "properties": {
    "event.name": { "const": "ripgrep_fallback" },
    "event.timestamp": { "type": "string", "format": "date-time" },
    "common": { "$ref": "#/definitions/common" },
    "error": { "type": "string" }
  },
  "definitions": { "common": { "$ref": "#/common" } }
}
```

### gemini_cli.web_fetch_fallback_attempt

```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "gemini_cli.web_fetch_fallback_attempt",
  "type": "object",
  "required": ["event.name", "event.timestamp", "common", "reason"],
  "properties": {
    "event.name": { "const": "web_fetch_fallback_attempt" },
    "event.timestamp": { "type": "string", "format": "date-time" },
    "common": { "$ref": "#/definitions/common" },
    "reason": { "type": "string", "enum": ["private_ip", "primary_failed"] }
  },
  "definitions": { "common": { "$ref": "#/common" } }
}
```

---

## 8. Extensions

### gemini_cli.extension_install

```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "gemini_cli.extension_install",
  "type": "object",
  "required": [
    "event.name",
    "event.timestamp",
    "common",
    "extension_name",
    "extension_version",
    "extension_source",
    "status"
  ],
  "properties": {
    "event.name": { "const": "extension_install" },
    "event.timestamp": { "type": "string", "format": "date-time" },
    "common": { "$ref": "#/definitions/common" },
    "extension_name": { "type": "string" },
    "extension_version": { "type": "string" },
    "extension_source": { "type": "string" },
    "status": { "type": "string" }
  },
  "definitions": { "common": { "$ref": "#/common" } }
}
```

### gemini_cli.extension_uninstall

```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "gemini_cli.extension_uninstall",
  "type": "object",
  "required": [
    "event.name",
    "event.timestamp",
    "common",
    "extension_name",
    "status"
  ],
  "properties": {
    "event.name": { "const": "extension_uninstall" },
    "event.timestamp": { "type": "string", "format": "date-time" },
    "common": { "$ref": "#/definitions/common" },
    "extension_name": { "type": "string" },
    "status": { "type": "string" }
  },
  "definitions": { "common": { "$ref": "#/common" } }
}
```

### gemini_cli.extension_enable

```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "gemini_cli.extension_enable",
  "type": "object",
  "required": [
    "event.name",
    "event.timestamp",
    "common",
    "extension_name",
    "setting_scope"
  ],
  "properties": {
    "event.name": { "const": "extension_enable" },
    "event.timestamp": { "type": "string", "format": "date-time" },
    "common": { "$ref": "#/definitions/common" },
    "extension_name": { "type": "string" },
    "setting_scope": { "type": "string" }
  },
  "definitions": { "common": { "$ref": "#/common" } }
}
```

### gemini_cli.extension_disable

```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "gemini_cli.extension_disable",
  "type": "object",
  "required": [
    "event.name",
    "event.timestamp",
    "common",
    "extension_name",
    "setting_scope"
  ],
  "properties": {
    "event.name": { "const": "extension_disable" },
    "event.timestamp": { "type": "string", "format": "date-time" },
    "common": { "$ref": "#/definitions/common" },
    "extension_name": { "type": "string" },
    "setting_scope": { "type": "string" }
  },
  "definitions": { "common": { "$ref": "#/common" } }
}
```

### gemini_cli.extension_update

```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "gemini_cli.extension_update",
  "type": "object",
  "required": [
    "event.name",
    "event.timestamp",
    "common",
    "extension_name",
    "extension_version",
    "extension_previous_version",
    "extension_source",
    "status"
  ],
  "properties": {
    "event.name": { "const": "extension_update" },
    "event.timestamp": { "type": "string", "format": "date-time" },
    "common": { "$ref": "#/definitions/common" },
    "extension_name": { "type": "string" },
    "extension_version": { "type": "string" },
    "extension_previous_version": { "type": "string" },
    "extension_source": { "type": "string" },
    "status": { "type": "string" }
  },
  "definitions": { "common": { "$ref": "#/common" } }
}
```

---

## 9. Agent runs

### gemini_cli.agent.start

```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "gemini_cli.agent.start",
  "type": "object",
  "required": [
    "event.name",
    "event.timestamp",
    "common",
    "agent_id",
    "agent_name"
  ],
  "properties": {
    "event.name": { "const": "agent_start" },
    "event.timestamp": { "type": "string", "format": "date-time" },
    "common": { "$ref": "#/definitions/common" },
    "agent_id": { "type": "string" },
    "agent_name": { "type": "string" }
  },
  "definitions": { "common": { "$ref": "#/common" } }
}
```

### gemini_cli.agent.finish

```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "gemini_cli.agent.finish",
  "type": "object",
  "required": [
    "event.name",
    "event.timestamp",
    "common",
    "agent_id",
    "agent_name",
    "duration_ms",
    "turn_count",
    "terminate_reason"
  ],
  "properties": {
    "event.name": { "const": "agent_finish" },
    "event.timestamp": { "type": "string", "format": "date-time" },
    "common": { "$ref": "#/definitions/common" },
    "agent_id": { "type": "string" },
    "agent_name": { "type": "string" },
    "duration_ms": { "type": "integer" },
    "turn_count": { "type": "integer" },
    "terminate_reason": { "type": "string" }
  },
  "definitions": { "common": { "$ref": "#/common" } }
}
```

### gemini_cli.agent.recovery_attempt

```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "gemini_cli.agent.recovery_attempt",
  "type": "object",
  "required": [
    "event.name",
    "event.timestamp",
    "common",
    "agent_id",
    "agent_name",
    "reason",
    "duration_ms",
    "success",
    "turn_count"
  ],
  "properties": {
    "event.name": { "const": "agent_recovery_attempt" },
    "event.timestamp": { "type": "string", "format": "date-time" },
    "common": { "$ref": "#/definitions/common" },
    "agent_id": { "type": "string" },
    "agent_name": { "type": "string" },
    "reason": { "type": "string" },
    "duration_ms": { "type": "integer" },
    "success": { "type": "boolean" },
    "turn_count": { "type": "integer" }
  },
  "definitions": { "common": { "$ref": "#/common" } }
}
```

---

## 10. IDE

### gemini_cli.ide_connection

```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "gemini_cli.ide_connection",
  "type": "object",
  "required": ["event.name", "event.timestamp", "common", "connection_type"],
  "properties": {
    "event.name": { "const": "ide_connection" },
    "event.timestamp": { "type": "string", "format": "date-time" },
    "common": { "$ref": "#/definitions/common" },
    "connection_type": { "type": "string" }
  },
  "definitions": { "common": { "$ref": "#/common" } }
}
```

---

## 11. UI

### kitty_sequence_overflow

```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "kitty_sequence_overflow",
  "type": "object",
  "required": [
    "event.name",
    "event.timestamp",
    "common",
    "sequence_length",
    "truncated_sequence"
  ],
  "properties": {
    "event.name": { "const": "kitty_sequence_overflow" },
    "event.timestamp": { "type": "string", "format": "date-time" },
    "common": { "$ref": "#/definitions/common" },
    "sequence_length": { "type": "integer" },
    "truncated_sequence": { "type": "string" }
  },
  "definitions": { "common": { "$ref": "#/common" } }
}
```

---

## 12. GenAI semantic event

### gen_ai.client.inference.operation.details

```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "gen_ai.client.inference.operation.details",
  "type": "object",
  "required": ["event.name", "event.timestamp", "common"],
  "properties": {
    "event.name": { "const": "gen_ai.client.inference.operation.details" },
    "event.timestamp": { "type": "string", "format": "date-time" },
    "common": { "$ref": "#/definitions/common" },
    "gen_ai.request.model": { "type": "string" },
    "gen_ai.provider.name": { "type": "string" },
    "gen_ai.operation.name": { "type": "string" },
    "gen_ai.input.messages": { "type": "string" },
    "gen_ai.output.messages": { "type": "string" },
    "gen_ai.response.finish_reasons": {
      "type": "array",
      "items": { "type": "string" }
    },
    "gen_ai.usage.input_tokens": { "type": "integer" },
    "gen_ai.usage.output_tokens": { "type": "integer" },
    "gen_ai.request.temperature": { "type": "number" },
    "gen_ai.request.top_p": { "type": "number" },
    "gen_ai.request.top_k": { "type": "integer" },
    "gen_ai.request.max_tokens": { "type": "integer" },
    "gen_ai.system_instructions": { "type": "string" },
    "server.address": { "type": "string" },
    "server.port": { "type": "integer" }
  },
  "definitions": { "common": { "$ref": "#/common" } }
}
```

```

```
