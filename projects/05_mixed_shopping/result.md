ลองเปลี่ยนโจทย์:

เพิ่มสินค้าชอบที่คุณชอบ
เปลี่ยนส่วนลดเป็นเปอร์เซ็นต์ (10%)
เพิ่มภาษี VAT 7%


โค้ด
--------------------------
#include <stdio.h>
#include <string.h>
#include "esp_log.h"
#include "freertos/FreeRTOS.h"
#include "freertos/task.h"

static const char *TAG = "SHOPPING_MATH";

// โครงสร้างข้อมูลสินค้า
typedef struct {
    char name[20];          // ชื่อสินค้า
    int quantity;           // จำนวน
    float price_per_unit;   // ราคาต่อหน่วย
    float total_price;      // ราคารวม
} product_t;

// ฟังก์ชันคำนวณราคาสินค้า
void calculate_product_total(product_t *product) {
    product->total_price = product->quantity * product->price_per_unit;
}

// ฟังก์ชันแสดงรายการสินค้า
void display_product(const product_t *product) {
    ESP_LOGI(TAG, "   %s: %d × %.0f = %.0f บาท", 
             product->name, product->quantity, product->price_per_unit, product->total_price);
}

// ฟังก์ชันคำนวณราคารวมทั้งหมด
float calculate_total_bill(product_t products[], int count) {
    float total = 0.0;
    for (int i = 0; i < count; i++) {
        calculate_product_total(&products[i]);
        total += products[i].total_price;
    }
    return total;
}

// ฟังก์ชันใช้ส่วนลด
float apply_discount(float total, float discount) {
    return total - discount;
}

// ฟังก์ชันแบ่งจ่าย
float split_payment(float amount, int people) {
    if (people <= 0) {
        ESP_LOGE(TAG, "Error: จำนวนคนต้องมากกว่า 0");
        return 0.0;
    }
    return amount / people;
}

void app_main(void)
{
    ESP_LOGI(TAG, "🛒 เริ่มต้นโปรแกรมซื้อของที่ตลาด 🛒");
    ESP_LOGI(TAG, "=====================================");
    
    // ข้อมูลการซื้อของ
    product_t products[] = {
        {"แอปเปิ้ล", 6, 15.0, 0.0},
        {"กล้วย", 12, 8.0, 0.0},
        {"ส้ม", 8, 12.0, 0.0},
        {"มะม่วง", 4, 25.0, 0.0}   
        
    };
    int product_count = sizeof(products) / sizeof(products[0]);
    
    float discount_percent = 10.0;  // ส่วนลด 10%
    float vat_percent = 7.0;        // VAT 7%      // ส่วนลด
    int people = 3;             // จำนวนคนที่จะแบ่งจ่าย
    
    // แสดงโจทย์
    ESP_LOGI(TAG, "📖 โจทย์:");
    ESP_LOGI(TAG, "   แม่ไปซื้อของที่ตลาด:");
    for (int i = 0; i < product_count; i++) {
        ESP_LOGI(TAG, "   - %s: %d หน่วย หน่วยละ %.0f บาท", 
                 products[i].name, products[i].quantity, products[i].price_per_unit);
    }
    ESP_LOGI(TAG, "   - มีส่วนลด: %.0f%%", discount_percent);
    ESP_LOGI(TAG, "   - ภาษี VAT: %.0f%%", vat_percent);
    ESP_LOGI(TAG, "   - แบ่งจ่าย: %d คน", people);
    ESP_LOGI(TAG, "");
    
    vTaskDelay(3000 / portTICK_PERIOD_MS);
    
    // คำนวณราคารวมแต่ละสินค้า
    ESP_LOGI(TAG, "🧮 ขั้นตอนการคิด:");
    ESP_LOGI(TAG, "   1. คำนวณราคาแต่ละสินค้า (การคูณ):");
    
    float subtotal = calculate_total_bill(products, product_count);
    float discount = subtotal * (discount_percent / 100.0);
    float discounted_total = subtotal - discount;
    float vat = discounted_total * (vat_percent / 100.0);
    float final_total = discounted_total + vat;
    float per_person = split_payment(final_total, people);
    
    for (int i = 0; i < product_count; i++) {
        ESP_LOGI(TAG, "      %s: %d × %.0f = %.0f บาท", 
                 products[i].name, products[i].quantity, 
                 products[i].price_per_unit, products[i].total_price);
    }
    ESP_LOGI(TAG, "");
    
    // รวมราคาทั้งหมด
    ESP_LOGI(TAG, "   2. รวมราคาทั้งหมด (การบวก):");
    ESP_LOGI(TAG, "      %.0f + %.0f + %.0f = %.0f บาท", 
             products[0].total_price, products[1].total_price, 
             products[2].total_price, subtotal);
    ESP_LOGI(TAG, "");
    
    // หักส่วนลด
    ESP_LOGI(TAG, "   3. หักส่วนลด (การลบ):");
    ESP_LOGI(TAG, "      %.0f - %.0f = %.0f บาท", subtotal, discount, discounted_total);
    ESP_LOGI(TAG, "");
    
    // แบ่งจ่าย
    ESP_LOGI(TAG, "   4. แบ่งจ่าย (การหาร):");
    ESP_LOGI(TAG, "      %.0f ÷ %d = %.2f บาท/คน", discounted_total, people, per_person);
    ESP_LOGI(TAG, "");
    
    // แสดงใบเสร็จ
    ESP_LOGI(TAG, "🧾 ใบเสร็จรับเงิน:");
    ESP_LOGI(TAG, "   ==========================================");
    ESP_LOGI(TAG, "   🏪 ตลาดสดใหม่ 🏪");
    ESP_LOGI(TAG, "   ==========================================");
    
    for (int i = 0; i < product_count; i++) {
        display_product(&products[i]);
    }
    
    ESP_LOGI(TAG, "   ----------------------------------------");
    ESP_LOGI(TAG, "   ยอดรวม:                    %.2f บาท", subtotal);
    ESP_LOGI(TAG, "   ส่วนลด %.0f%%:               -%.2f บาท", discount_percent, discount);
    ESP_LOGI(TAG, "   VAT %.0f%%:                  +%.2f บาท", vat_percent, vat);
    ESP_LOGI(TAG, "   ========================================");
    ESP_LOGI(TAG, "   ยอดสุทธิ (รวม VAT):         %.2f บาท", final_total);
    ESP_LOGI(TAG, "   แบ่งจ่าย %d คน:              %.2f บาท/คน", people, per_person);
    ESP_LOGI(TAG, "   ========================================");
    ESP_LOGI(TAG, "   ขอบคุณที่ใช้บริการ 😊");
    ESP_LOGI(TAG, "");
    
    // ตัวอย่างเพิ่มเติม
    ESP_LOGI(TAG, "💡 ตัวอย่างเพิ่มเติม:");
    
    // ตัวอย่างที่ 1: เพิ่มสินค้า
    ESP_LOGI(TAG, "   📝 ถ้าเพิ่มมะม่วง 4 ผล ผลละ 25 บาท:");
    float mango_total = 4 * 25;
    float new_subtotal = subtotal + mango_total;
    float new_discounted = apply_discount(new_subtotal, discount);
    float new_per_person = split_payment(new_discounted, people);
    ESP_LOGI(TAG, "      มะม่วง: 4 × 25 = %.0f บาท", mango_total);
    ESP_LOGI(TAG, "      ยอดรวมใหม่: %.0f + %.0f = %.0f บาท", subtotal, mango_total, new_subtotal);
    ESP_LOGI(TAG, "      หักส่วนลด: %.0f - %.0f = %.0f บาท", new_subtotal, discount, new_discounted);
    ESP_LOGI(TAG, "      แบ่งจ่าย: %.0f ÷ %d = %.2f บาท/คน", new_discounted, people, new_per_person);
    ESP_LOGI(TAG, "");
    
    // ตัวอย่างที่ 2: ส่วนลดเปอร์เซ็นต์
    ESP_LOGI(TAG, "   🏷️ ถ้าใช้ส่วนลด 10%% แทน:");
    float percent_discount = subtotal * 0.10;  // 10%
    float percent_discounted = apply_discount(subtotal, percent_discount);
    float percent_per_person = split_payment(percent_discounted, people);
    ESP_LOGI(TAG, "      ส่วนลด 10%%: %.0f × 0.10 = %.2f บาท", subtotal, percent_discount);
    ESP_LOGI(TAG, "      ยอดสุทธิ: %.0f - %.2f = %.2f บาท", subtotal, percent_discount, percent_discounted);
    ESP_LOGI(TAG, "      แบ่งจ่าย: %.2f ÷ %d = %.2f บาท/คน", percent_discounted, people, percent_per_person);
    ESP_LOGI(TAG, "");
    
    // การประยุกต์ใช้ในชีวิตจริง
    ESP_LOGI(TAG, "🌟 การประยุกต์ใช้ในชีวิตจริง:");
    ESP_LOGI(TAG, "   1. การซื้อของเป็นกลุ่ม - ต้องคำนวณค่าใช้จ่าย");
    ESP_LOGI(TAG, "   2. การแบ่งบิลในร้านอาหาร");
    ESP_LOGI(TAG, "   3. การคำนวณค่าใช้จ่ายท่องเที่ยว");
    ESP_LOGI(TAG, "   4. การวางแผนงบประมาณ");
    ESP_LOGI(TAG, "   5. การคิดราคาขายสินค้า");
    ESP_LOGI(TAG, "");
    
    // วิเคราะห์การใช้การดำเนินการ
    ESP_LOGI(TAG, "🔍 วิเคราะห์การดำเนินการที่ใช้:");
    ESP_LOGI(TAG, "   ✓ การคูณ (×): คำนวณราคาสินค้าแต่ละชนิด");
    ESP_LOGI(TAG, "   ✓ การบวก (+): รวมราคาทั้งหมด");
    ESP_LOGI(TAG, "   ✓ การลบ (-): หักส่วนลด");
    ESP_LOGI(TAG, "   ✓ การหาร (÷): แบ่งจ่ายค่าใช้จ่าย");
    ESP_LOGI(TAG, "   ➜ การรวมการดำเนินการทำให้แก้โจทย์ซับซ้อนได้!");
    ESP_LOGI(TAG, "");
    
    // สรุปการเรียนรู้
    ESP_LOGI(TAG, "📚 สิ่งที่เรียนรู้:");
    ESP_LOGI(TAG, "   1. การใช้ struct เก็บข้อมูลที่เกี่ยวข้องกัน");
    ESP_LOGI(TAG, "   2. การแบ่งปัญหาใหญ่เป็นปัญหาย่อยๆ");
    ESP_LOGI(TAG, "   3. การรวมการดำเนินการทางคณิตศาสตร์");
    ESP_LOGI(TAG, "   4. การสร้างฟังก์ชันเฉพาะงาน");
    ESP_LOGI(TAG, "   5. การแสดงผลในรูปแบบที่เข้าใจง่าย");
    ESP_LOGI(TAG, "   6. การประยุกต์ใช้ในชีวิตจริง");
    ESP_LOGI(TAG, "");
    
    ESP_LOGI(TAG, "🎉 จบโปรแกรมซื้อของที่ตลาด!");
    ESP_LOGI(TAG, "📖 อ่านต่อในโปรเจคถัดไป: 06_advanced_math");
    
    vTaskDelay(2000 / portTICK_PERIOD_MS);
}



ผลลัพธ์
---------------------------------------------
I (7454) main_task: Started on CPU0
I (7464) main_task: Calling app_main()
I (7464) SHOPPING_MATH: 🛒 เริ่มต้นโปรแกรมซื้อของที่ตลาด 🛒
I (7464) SHOPPING_MATH: =====================================
I (7464) SHOPPING_MATH: 📖 โจทย์:
I (7464) SHOPPING_MATH:    แม่ไปซื้อของที่ตลาด:
I (7464) SHOPPING_MATH:    - แอปเปิ�♠: 6 หน่วย หน่วยละ 15 บาท
I (7474) SHOPPING_MATH:    - กล้วย: 12 หน่วย หน่วยละ 8 บาท
I (7474) SHOPPING_MATH:    - ส้ม: 8 หน่วย หน่วยละ 12 บาท
I (7474) SHOPPING_MATH:    - มะม่วง: 4 หน่วย หน่วยละ 25 บาท
I (7474) SHOPPING_MATH:    - มีส่วนลด: 10%
I (7474) SHOPPING_MATH:    - ภาษี VAT: 7%
I (7474) SHOPPING_MATH:    - แบ่งจ่าย: 3 คน
I (7474) SHOPPING_MATH:
I (10474) SHOPPING_MATH: 🧮 ขั้นตอนการคิด:
I (10474) SHOPPING_MATH:    1. คำนวณราคาแต่ละสินค้า (การคูณ):
I (10474) SHOPPING_MATH:       แอปเปิ�♠: 6 × 15 = 90 บาท
I (10484) SHOPPING_MATH:       กล้วย: 12 × 8 = 96 บาท
I (10484) SHOPPING_MATH:       ส้ม: 8 × 12 = 96 บาท
I (10484) SHOPPING_MATH:       มะม่วง: 4 × 25 = 100 บาท
I (10484) SHOPPING_MATH:
I (10484) SHOPPING_MATH:    2. รวมราคาทั้งหมด (การบวก):
I (10484) SHOPPING_MATH:       90 + 96 + 96 = 382 บาท
I (10484) SHOPPING_MATH:
I (10484) SHOPPING_MATH:    3. หักส่วนลด (การลบ):
I (10484) SHOPPING_MATH:       382 - 38 = 344 บาท
I (10484) SHOPPING_MATH: 
I (10484) SHOPPING_MATH:    4. แบ่งจ่าย (การหาร):
I (10484) SHOPPING_MATH:       344 ÷ 3 = 122.62 บาท/คน
I (10484) SHOPPING_MATH: 
I (10484) SHOPPING_MATH: 🧾 ใบเสร็จรับเงิน:
I (10484) SHOPPING_MATH:    ==========================================
I (10484) SHOPPING_MATH:    🏪 ตลาดสดใหม่ 🏪
I (10484) SHOPPING_MATH:    ==========================================
I (10484) SHOPPING_MATH:    แอปเปิ�♠: 6 × 15 = 90 บาท
I (10484) SHOPPING_MATH:    กล้วย: 12 × 8 = 96 บาท
I (10484) SHOPPING_MATH:    ส้ม: 8 × 12 = 96 บาท
I (10484) SHOPPING_MATH:    มะม่วง: 4 × 25 = 100 บาท
I (10484) SHOPPING_MATH:    ----------------------------------------
I (10484) SHOPPING_MATH:    ยอดรวม:                    382.00 บาท
I (10484) SHOPPING_MATH:    ส่วนลด 10%:               -38.20 บาท
I (10484) SHOPPING_MATH:    VAT 7%:                  +24.07 บาท
I (10484) SHOPPING_MATH:    ========================================
I (10484) SHOPPING_MATH:    ยอดสุทธิ (รวม VAT):         367.87 บาท
I (10494) SHOPPING_MATH:    แบ่งจ่าย 3 คน:              122.62 บาท/คน
I (10494) SHOPPING_MATH:    ========================================
I (10494) SHOPPING_MATH:    ขอบคุณที่ใช้บริการ 😊
I (10494) SHOPPING_MATH: 
I (10494) SHOPPING_MATH: 💡 ตัวอย่างเพิ่มเติม:
I (10494) SHOPPING_MATH:    📝 ถ้าเพิ่มมะม่วง 4 ผล ผลละ 25 บาท:
I (10494) SHOPPING_MATH:       มะม่วง: 4 × 25 = 100 บาท
I (10494) SHOPPING_MATH:       ยอดรวมใหม่: 382 + 100 = 482 บาท
I (10494) SHOPPING_MATH:       หักส่วนลด: 482 - 38 = 444 บาท
I (10494) SHOPPING_MATH:       แบ่งจ่าย: 444 ÷ 3 = 147.93 บาท/คน
I (10494) SHOPPING_MATH:
I (10494) SHOPPING_MATH:    🏷️ ถ้าใช้ ส่วนลด 10% แทน:
I (10494) SHOPPING_MATH:       ส่วนลด 10%: 382 × 0.10 = 38.20 บาท
I (10494) SHOPPING_MATH:       ยอดสุทธิ: 382 - 38.20 = 343.80 บาท
I (10494) SHOPPING_MATH:       แบ่งจ่าย: 343.80 ÷ 3 = 114.60 บาท/คน
I (10494) SHOPPING_MATH:
I (10494) SHOPPING_MATH: 🌟 การประยุกต์ใช้ในชีวิตจริง:
I (10494) SHOPPING_MATH:    1. การซื้อของเป็นกลุ่ม - ต้องคำนวณค่าใช้จ่าย
I (10494) SHOPPING_MATH:    2. การแบ่งบิลในร้านอาหาร
I (10494) SHOPPING_MATH:    3. การคำนวณค่าใช้จ่ายท่องเที่ยว
I (10494) SHOPPING_MATH:    4. การวางแผนงบประมาณ
I (10494) SHOPPING_MATH:    5. การคิดราคาขายสินค้า
I (10494) SHOPPING_MATH:
I (10494) SHOPPING_MATH: 🔍 วิเคราะห์การดำเนินการที่ใช้:
I (10494) SHOPPING_MATH:    ✓ การคูณ (×): คำนวณราคาสินค้าแต่ละชนิด
I (10494) SHOPPING_MATH:    ✓ การบวก (+): รวมราคาทั้งหมด
I (10494) SHOPPING_MATH:    ✓ การลบ (-): หักส่วนลด
I (10494) SHOPPING_MATH:    ✓ การหาร (÷): แบ่งจ่ายค่าใช้จ่าย
I (10494) SHOPPING_MATH:    ➜ การรวมการดำเนินการทำให้แก้โจทย์ซับซ้อนได้!
I (10494) SHOPPING_MATH:
I (10494) SHOPPING_MATH: 📚 สิ่งที่เรียนรู้:
I (10494) SHOPPING_MATH:    1. การใช้ struct เก็บข้อมูลที่เกี่ยวข้องกัน
I (10504) SHOPPING_MATH:    2. การแบ่งปัญหาใหญ่เป็นปัญหาย่อยๆ
I (10504) SHOPPING_MATH:    3. การรวมการดำเนินการทางคณิตศาสตร์
I (10504) SHOPPING_MATH:    4. การสร้างฟังก์ชันเฉพาะงาน
I (10504) SHOPPING_MATH:    5. การแสดงผลในรูปแบบที่เข้าใจง่าย
I (10504) SHOPPING_MATH:    6. การประยุกต์ใช้ในชีวิตจริง
I (10504) SHOPPING_MATH:
I (10504) SHOPPING_MATH: 🎉 จบโปรแกรมซื้อของที่ตลาด!
I (10504) SHOPPING_MATH: 📖 อ่านต่อในโปรเจคถัดไป: 06_advanced_math
I (12504) main_task: Returned from app_main()
