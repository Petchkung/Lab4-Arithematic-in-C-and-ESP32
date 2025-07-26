ลองเพิ่ม:

ตรวจสอบ email format
ตรวจสอบเบอร์โทรศัพท์
ตรวจสอบรหัสประจำตัวประชาชน
สร้างระบบ retry mechanism


โค้ด
-----------------------
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <math.h>
#include <float.h>
#include "esp_log.h"
#include "freertos/FreeRTOS.h"
#include "freertos/task.h"

static const char *TAG = "ERROR_HANDLING";

typedef enum {
    ERROR_NONE = 0,
    ERROR_DIVISION_BY_ZERO,
    ERROR_INVALID_INPUT,
    ERROR_OUT_OF_RANGE,
    ERROR_NEGATIVE_VALUE,
    ERROR_OVERFLOW,
    ERROR_UNDERFLOW
} error_code_t;

typedef struct {
    double result;
    error_code_t error;
    char message[100];
} calculation_result_t;

void show_ascii_art(error_code_t error) {
    switch(error) {
        case ERROR_NONE:
            ESP_LOGI(TAG, "   ✅ SUCCESS");
            ESP_LOGI(TAG, "     🎉🎉🎉");
            ESP_LOGI(TAG, "    สำเร็จแล้ว!");
            break;
        case ERROR_DIVISION_BY_ZERO:
            ESP_LOGI(TAG, "   🍕 ÷ 0 = ❌");
            ESP_LOGI(TAG, "   😱 โอ้ะโอ!");
            ESP_LOGI(TAG, "  ไม่มีลูกค้า!");
            break;
        case ERROR_INVALID_INPUT:
            ESP_LOGI(TAG, "   📝 ABC บาท?");
            ESP_LOGI(TAG, "   🤔 งง...");
            ESP_LOGI(TAG, "  ตัวเลขหายไป");
            break;
        case ERROR_OUT_OF_RANGE:
            ESP_LOGI(TAG, "   📈 ∞∞∞∞∞");
            ESP_LOGI(TAG, "   😵 เกินขีด!");
            ESP_LOGI(TAG, "  ใหญ่เกินไป");
            break;
        default:
            ESP_LOGI(TAG, "   ❓ ERROR");
            ESP_LOGI(TAG, "   🔧 แก้ไข");
            ESP_LOGI(TAG, "  ต้องตรวจสอบ");
    }
}

calculation_result_t safe_divide(double dividend, double divisor, const char* context) {
    calculation_result_t result = {0};

    ESP_LOGI(TAG, "\n🔍 ตรวจสอบการหาร: %s", context);
    ESP_LOGI(TAG, "📊 %.2f ÷ %.2f = ?", dividend, divisor);

    if (divisor == 0.0) {
        result.error = ERROR_DIVISION_BY_ZERO;
        snprintf(result.message, sizeof(result.message), "❌ หารด้วยศูนย์ไม่ได้!");
        ESP_LOGE(TAG, "%s", result.message);
        show_ascii_art(ERROR_DIVISION_BY_ZERO);
        ESP_LOGI(TAG, "💡 ตรวจสอบจำนวนลูกค้าก่อนแบ่งพิซซ่า");
        return result;
    }

    result.result = dividend / divisor;
    if (isinf(result.result)) {
        result.error = ERROR_OVERFLOW;
        snprintf(result.message, sizeof(result.message), "⚠️ ค่าผลลัพธ์เป็น Infinity!");
        ESP_LOGW(TAG, "%s", result.message);
        return result;
    }

    result.error = ERROR_NONE;
    snprintf(result.message, sizeof(result.message), "✅ สำเร็จ: %.2f ÷ %.2f = %.2f", dividend, divisor, result.result);
    ESP_LOGI(TAG, "%s", result.message);
    show_ascii_art(ERROR_NONE);

    return result;
}

calculation_result_t validate_money(double amount, const char* description) {
    calculation_result_t result = {0};

    ESP_LOGI(TAG, "\n💰 ตรวจสอบเงิน: %s", description);
    ESP_LOGI(TAG, "💵 จำนวน: %.2f บาท", amount);

    if (amount < 0) {
        result.error = ERROR_NEGATIVE_VALUE;
        snprintf(result.message, sizeof(result.message), "❌ จำนวนเงินต้องไม่ติดลบ!");
        ESP_LOGE(TAG, "%s", result.message);
        ESP_LOGI(TAG, "💡 ตรวจสอบการคิดเงินใหม่");
        return result;
    }

    if (amount > 1000000000000.0) {
        result.error = ERROR_OUT_OF_RANGE;
        snprintf(result.message, sizeof(result.message), "⚠️ จำนวนเงินเกินขีดจำกัด!");
        ESP_LOGW(TAG, "%s", result.message);
        show_ascii_art(ERROR_OUT_OF_RANGE);
        ESP_LOGI(TAG, "💡 แนะนำ: ใช้ระบบธนาคารกลาง");
        return result;
    }

    double rounded = round(amount * 100) / 100;
    if (fabs(amount - rounded) > 0.001) {
        ESP_LOGW(TAG, "⚠️ ปัดเศษจาก %.4f → %.2f บาท", amount, rounded);
        amount = rounded;
    }

    result.result = amount;
    result.error = ERROR_NONE;
    snprintf(result.message, sizeof(result.message), "✅ เงินถูกต้อง: %.2f บาท", amount);
    ESP_LOGI(TAG, "%s", result.message);

    return result;
}

calculation_result_t validate_number(const char* input, const char* field_name) {
    calculation_result_t result = {0};

    ESP_LOGI(TAG, "\n🔢 ตรวจสอบตัวเลข: %s", field_name);
    ESP_LOGI(TAG, "📝 ข้อมูลที่ป้อน: '%s'", input);

    if (input == NULL || strlen(input) == 0) {
        result.error = ERROR_INVALID_INPUT;
        snprintf(result.message, sizeof(result.message), "❌ ไม่มีข้อมูล!");
        ESP_LOGE(TAG, "%s", result.message);
        return result;
    }

    char* endptr;
    double value = strtod(input, &endptr);

    if (*endptr != '\0') {
        result.error = ERROR_INVALID_INPUT;
        snprintf(result.message, sizeof(result.message), "❌ '%s' ไม่ใช่ตัวเลข!", input);
        ESP_LOGE(TAG, "%s", result.message);
        show_ascii_art(ERROR_INVALID_INPUT);
        ESP_LOGI(TAG, "💡 ใช้เฉพาะตัวเลข 0-9 และจุดทศนิยม");
        return result;
    }

    if (isnan(value) || isinf(value)) {
        result.error = ERROR_INVALID_INPUT;
        snprintf(result.message, sizeof(result.message), "❌ ตัวเลขไม่ถูกต้อง!");
        ESP_LOGE(TAG, "%s", result.message);
        return result;
    }

    result.result = value;
    result.error = ERROR_NONE;
    snprintf(result.message, sizeof(result.message), "✅ ตัวเลขถูกต้อง: %.2f", value);
    ESP_LOGI(TAG, "%s", result.message);

    return result;
}

calculation_result_t calculate_interest(double principal, double rate, int years) {
    calculation_result_t result = {0};

    ESP_LOGI(TAG, "\n🏦 คำนวณดอกเบี้ย");
    ESP_LOGI(TAG, "💰 เงินต้น: %.2f บาท", principal);
    ESP_LOGI(TAG, "📈 อัตราดอกเบี้ย: %.2f%% ต่อปี", rate);
    ESP_LOGI(TAG, "⏰ ระยะเวลา: %d ปี", years);

    if (principal <= 0) {
        result.error = ERROR_NEGATIVE_VALUE;
        snprintf(result.message, sizeof(result.message), "❌ เงินต้นต้องมากกว่าศูนย์!");
        ESP_LOGE(TAG, "%s", result.message);
        return result;
    }

    if (rate < -100 || rate > 100) {
        result.error = ERROR_OUT_OF_RANGE;
        snprintf(result.message, sizeof(result.message), "❌ อัตราดอกเบี้ยไม่เหมาะสม!");
        ESP_LOGE(TAG, "%s", result.message);
        ESP_LOGI(TAG, "💡 ใช้ -100%% ถึง 100%% เท่านั้น");
        return result;
    }

    if (years < 0 || years > 100) {
        result.error = ERROR_OUT_OF_RANGE;
        snprintf(result.message, sizeof(result.message), "❌ ระยะเวลาไม่เหมาะสม!");
        ESP_LOGE(TAG, "%s", result.message);
        return result;
    }

    double interest = principal * (rate / 100.0) * years;
    double total = principal + interest;

    if (total > DBL_MAX / 2) {
        result.error = ERROR_OVERFLOW;
        snprintf(result.message, sizeof(result.message), "⚠️ ผลลัพธ์ใหญ่เกินไป!");
        ESP_LOGW(TAG, "%s", result.message);
        return result;
    }

    result.result = total;
    result.error = ERROR_NONE;
    snprintf(result.message, sizeof(result.message), "✅ ดอกเบี้ย: %.2f, รวม: %.2f", interest, total);
    ESP_LOGI(TAG, "%s", result.message);

    return result;
}

void pizza_shop_scenario(void) {
    ESP_LOGI(TAG, "\n🍕 === ร้านพิซซ่า ===");
    safe_divide(12, 4, "แบ่งพิซซ่าให้ลูกค้า 4 คน");
    vTaskDelay(pdMS_TO_TICKS(2000));

    safe_divide(12, 0, "แบ่งพิซซ่าให้ลูกค้า 0 คน");
    vTaskDelay(pdMS_TO_TICKS(2000));

    ESP_LOGI(TAG, "\n🌞 ฝนหยุดแล้ว! ลูกค้ามา 3 คน");
    safe_divide(12, 4, "แบ่งพิซซ่าให้ลูกค้า 3 คน");
}

void shop_scenario(void) {
    ESP_LOGI(TAG, "\n🛒 === ร้านขายของ ===");
    validate_number("ABC", "ราคาสินค้า");
    vTaskDelay(pdMS_TO_TICKS(2000));

    validate_number("12.50", "ราคาสินค้า");
    vTaskDelay(pdMS_TO_TICKS(1000));

    validate_money(-50.0, "เงินทอน");
    vTaskDelay(pdMS_TO_TICKS(2000));

    validate_money(25.75, "เงินทอน");
}

void bank_scenario(void) {
    ESP_LOGI(TAG, "\n🏦 === ธนาคาร ===");
    calculate_interest(100000, 2.5, 5);
    vTaskDelay(pdMS_TO_TICKS(2000));

    calculate_interest(100000, -5.0, 5);
    vTaskDelay(pdMS_TO_TICKS(2000));

    validate_money(999999999999.0, "เงินฝาก");
    vTaskDelay(pdMS_TO_TICKS(2000));

    calculate_interest(100000, 3.0, 10);
}

void show_error_handling_summary(void) {
    ESP_LOGI(TAG, "\n📚 === สรุปข้อผิดพลาด ===");
    ESP_LOGI(TAG, "╔══════════════════════════════════════╗");
    ESP_LOGI(TAG, "║ 🚫 Division by Zero  - หารด้วยศูนย์ ║");
    ESP_LOGI(TAG, "║ 📝 Invalid Input      - ป้อนผิด     ║");
    ESP_LOGI(TAG, "║ 📊 Out of Range       - เกินขอบเขต  ║");
    ESP_LOGI(TAG, "║ ➖ Negative Value      - ค่าติดลบ     ║");
    ESP_LOGI(TAG, "║ ⬆️ Overflow            - ล้นค่าระบบ ║");
    ESP_LOGI(TAG, "╚══════════════════════════════════════╝");

    ESP_LOGI(TAG, "\n🛡️ หลักการจัดการข้อผิดพลาด:");
    ESP_LOGI(TAG, "✅ ตรวจสอบข้อมูลก่อนคำนวณ");
    ESP_LOGI(TAG, "✅ แจ้งเตือนแบบเข้าใจง่าย");
    ESP_LOGI(TAG, "✅ ให้คำแนะนำแก้ปัญหา");
    ESP_LOGI(TAG, "✅ ป้องกัน crash หรือ hang");
    ESP_LOGI(TAG, "✅ ใช้ enum + struct คุมสถานะ");
}

void app_main(void) {
    ESP_LOGI(TAG, "🚀 เริ่มต้นระบบจัดการข้อผิดพลาด!");
    vTaskDelay(pdMS_TO_TICKS(1000));

    pizza_shop_scenario();
    vTaskDelay(pdMS_TO_TICKS(3000));

    shop_scenario();
    vTaskDelay(pdMS_TO_TICKS(3000));

    bank_scenario();
    vTaskDelay(pdMS_TO_TICKS(3000));

    show_error_handling_summary();

    ESP_LOGI(TAG, "\n✅ เสร็จสิ้น! พร้อมเขียนโปรแกรมปลอดภัยแล้ว!");
}


ผลลัพธ์
---------------------------------
I (12033) ERROR_HANDLING: 
🍕 === ร้านพิซซ่า ===
I (12033) ERROR_HANDLING: 
🔍 ตรวจสอบการหาร: แบ่งพิซซ่าให้ลูกค้า 4 คน
I (12033) ERROR_HANDLING: 📊 12.00 ÷ 4.00 = ?
I (12033) ERROR_HANDLING: ✅ สำเร็จ: 12.00 ÷ 4.00 = 3.00
I (12033) ERROR_HANDLING:    ✅ SUCCESS
I (12043) ERROR_HANDLING:      🎉🎉🎉
I (12043) ERROR_HANDLING:     สำเร็จแล้ว!
I (14043) ERROR_HANDLING: 
🔍 ตรวจสอบการหาร: แบ่งพิซซ่าให้ลูกค้า 0 คน
I (14043) ERROR_HANDLING: 📊 12.00 ÷ 0.00 = ?
E (14043) ERROR_HANDLING: ❌ หารด้วยศูนย์ไม่ได้!
I (14043) ERROR_HANDLING:    🍕 ÷ 0 = ❌
I (14043) ERROR_HANDLING:    😱 โอ้ะโอ!
I (14043) ERROR_HANDLING:   ไม่มีลูกค้า!
I (14043) ERROR_HANDLING: 💡 ตรวจสอบจำนวนลูกค้าก่อนแบ่งพิซซ่า
I (16043) ERROR_HANDLING: 
🌞 ฝนหยุดแล้ว! ลูกค้ามา 3 คน
I (16043) ERROR_HANDLING:
🔍 ตรวจสอบการหาร: แบ่งพิซซ่าให้ลูกค้า 3 คน
I (16043) ERROR_HANDLING: 📊 12.00 ÷ 4.00 = ?
I (16043) ERROR_HANDLING: ✅ สำเร็จ: 12.00 ÷ 4.00 = 3.00
I (16043) ERROR_HANDLING:    ✅ SUCCESS
I (16043) ERROR_HANDLING:      🎉🎉🎉
I (16043) ERROR_HANDLING:     สำเร็จแล้ว!
I (19043) ERROR_HANDLING: 
🛒 === ร้านขายของ ===
I (19043) ERROR_HANDLING: 
🔢 ตรวจสอบตัวเลข: ราคาสินค้า
I (19043) ERROR_HANDLING: 📝 ข้อมูลที่ป้อน: 'ABC'
E (19043) ERROR_HANDLING: ❌ 'ABC' ไม่ใช่ตัวเลข!
I (19043) ERROR_HANDLING:    📝 ABC บาท?
I (19043) ERROR_HANDLING:    🤔 งง...
I (19043) ERROR_HANDLING:   ตัวเลขหายไป
I (19043) ERROR_HANDLING: 💡 ใช้เฉพาะตัวเลข 0-9 และจุดทศนิยม
I (21043) ERROR_HANDLING: 
🔢 ตรวจสอบตัวเลข: ราคาสินค้า
I (21043) ERROR_HANDLING: 📝 ข้อมูลที่ป้อน: '12.50'
I (21043) ERROR_HANDLING: ✅ ตัวเลขถูกต้อง: 12.50
I (22043) ERROR_HANDLING: 
💰 ตรวจสอบเงิน: เงินทอน
I (22043) ERROR_HANDLING: 💵 จำนวน: -50.00 บาท
E (22043) ERROR_HANDLING: ❌ จำนวนเงินต้องไม่ติดลบ!
I (22043) ERROR_HANDLING: 💡 ตรวจสอบการคิดเงินใหม่
I (24043) ERROR_HANDLING: 
💰 ตรวจสอบเงิน: เงินทอน
I (24043) ERROR_HANDLING: 💵 จำนวน: 25.75 บาท
I (24043) ERROR_HANDLING: ✅ เงินถูกต้อง: 25.75 บาท
I (27043) ERROR_HANDLING: 
🏦 === ธนาคาร ===
I (27043) ERROR_HANDLING:
🏦 คำนวณดอกเบี้ย
I (27043) ERROR_HANDLING: 💰 เงินต้น: 100000.00 บาท
I (27043) ERROR_HANDLING: 📈 อัตราดอกเบี้ย: 2.50% ต่อปี
I (27043) ERROR_HANDLING: ⏰ ระยะเวลา: 5 ปี
I (27043) ERROR_HANDLING: ✅ ดอกเบี้ย: 12500.00, รวม: 112500.00
I (29043) ERROR_HANDLING: 
🏦 คำนวณดอกเบี้ย
I (29043) ERROR_HANDLING: 💰 เงินต้น: 100000.00 บาท
I (29043) ERROR_HANDLING: 📈 อัตราดอกเบี้ย: -5.00% ต่อปี
I (29043) ERROR_HANDLING: ⏰ ระยะเวลา: 5 ปี
I (29043) ERROR_HANDLING: ✅ ดอกเบี้ย: -25000.00, รวม: 75000.00
I (31043) ERROR_HANDLING: 
💰 ตรวจสอบเงิน: เงินฝาก
I (31043) ERROR_HANDLING: 💵 จำนวน: 999999999999.00 บาท
I (31043) ERROR_HANDLING: ✅ เงินถูกต้อง: 999999999999.00 บาท
I (33043) ERROR_HANDLING: 
🏦 คำนวณดอกเบี้ย
I (33043) ERROR_HANDLING: 💰 เงินต้น: 100000.00 บาท
I (33043) ERROR_HANDLING: 📈 อัตราดอกเบี้ย: 3.00% ต่อปี
I (33043) ERROR_HANDLING: ⏰ ระยะเวลา: 10 ปี
I (33043) ERROR_HANDLING: ✅ ดอกเบี้ย: 30000.00, รวม: 130000.00
I (36043) ERROR_HANDLING: 
📚 === สรุปข้อผิดพลาด ===
I (36043) ERROR_HANDLING: ╔══════════════════════════════════════╗
I (36043) ERROR_HANDLING: ║ 🚫 Division by Zero  - หารด้วยศูนย์ ║
I (36043) ERROR_HANDLING: ║ 📝 Invalid Input      - ป้อนผิด     ║
I (36043) ERROR_HANDLING: ║ 📊 Out of Range       - เกินขอบเขต  ║
I (36043) ERROR_HANDLING: ║ ➖ Negative Value      - ค่าติดลบ     ║
I (36043) ERROR_HANDLING: ║ ⬆️ Overflow            - ล้นค่าระบบ ║
I (36043) ERROR_HANDLING: ╚══════════════════════════════════════╝
I (36043) ERROR_HANDLING:
🛡️ หลักการจัดการข้อผิดพลาด:
I (36043) ERROR_HANDLING: ✅ ตรวจสอบข้อมูลก่อนคำนวณ
I (36043) ERROR_HANDLING: ✅ แจ้งเตือนแบบเข้าใจง่าย
I (36043) ERROR_HANDLING: ✅ ให้คำแนะนำแก้ปัญหา
I (36043) ERROR_HANDLING: ✅ ป้องกัน crash หรือ hang
I (36043) ERROR_HANDLING: ✅ ใช้ enum + struct คุมสถานะ
I (36053) ERROR_HANDLING:
✅ เสร็จสิ้น! พร้อมเขียนโปรแกรมปลอดภัยแล้ว!
I (36053) main_task: Returned from app_main()
