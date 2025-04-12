# STM32 Timer Module 🕒

A hardware timer driver for STM32-based embedded systems, designed to track system uptime in milliseconds with high precision. Built for 10kHz resolution (0.1ms per tick) and tested using `Google Test` with fully mocked HAL peripherals via `FFF`.

---

## 📋 Overview

This module provides a lightweight and testable interface to read elapsed system time by wrapping STM32 HAL timer functionality in a consistent and validated format.

- Timer resolution: **0.1 ms per tick**
- Primary API: `timer_get_ms(float *ms)`
- Designed to run in real-time on STM32 targets and off-target in simulation

---

## 🧱 File Structure

```bash
.
├── timer.c / timer.h             # Core implementation of timer_get_ms
├── hal_timer_mock.c / .h        # HAL mock for STM32 timers (for GTest)
├── timer_test.cpp               # GTest suite with full mock-based validation
```

## 🛠 Dependencies
This module depends on:

STM32 HAL drivers — for integration with STM32 hardware timers

FFF — for mocking HAL function calls during testing

Google Test (GTest) — for writing and executing unit tests

rocketlib/include/common.h — provides project-wide enums like W_SUCCESS, W_FAILURE, etc.

These dependencies enable both embedded integration and fully off-target testability.

---

## ✅ Functionality
w_status_t timer_get_ms(float *ms)
Retrieves system uptime in milliseconds.

Behavior:
If ms == NULL: returns W_INVALID_PARAM

If timer is not running (HAL_TIM_State != BUSY) or not initialized: returns W_FAILURE

If successful: stores converted time (ticks * 0.1f) in *ms and returns W_SUCCESS

This function is designed to safely run in real-time and protect against invalid calls.

---

## 🧪 Unit Testing
Unit tests are implemented using Google Test and FFF (Fake Function Framework) to simulate the HAL environment off-target.

Test cases include:

❌ NULL pointer handling

❌ Uninitialized timer (htim2.Instance == NULL)

❌ Timer not running (wrong HAL state)

✅ Successful time retrieval (e.g., 1000 ticks → 100.0ms)

✅ Upper bound test (e.g., UINT32_MAX)
