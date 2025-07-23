
📝 แบบฝึกหัดที่ 1: เปลี่ยนจำนวนของเล่น
// หาบรรทัดนี้ในโค้ด:
int toys_at_home = 8;      // ของเล่นที่บ้าน
int toys_give_away = 3;    // ของเล่นที่แจก

// ลองเปลี่ยนเป็น:
int toys_at_home = 15;     // เพิ่มเป็น 15 ชิ้น
int toys_give_away = 7;    // แจกไป 7 ชิ้น
📝 แบบฝึกหัดที่ 2: เพิ่มการตรวจสอบ
เพิ่มการตรวจสอบว่าของเล่นพอแจกไหม:

// เพิ่มหลังบรรทัด int toys_remaining;
if (toys_at_home >= toys_give_away) {
    ESP_LOGI(TAG, "✅ ของเล่นพอแจก");
} else {
    ESP_LOGI(TAG, "❌ ของเล่นไม่พอ! ขาดไป %d ชิ้น", 
             toys_give_away - toys_at_home);
}
📝 แบบฝึกหัดที่ 3: เพิ่มของเล่นประเภทอื่น
เพิ่มตุ๊กตา และหุ่นยนต์:

int dolls = 5;        // ตุ๊กตา
int robots = 2;       // หุ่นยนต์
int total_toys = toys_at_home + dolls + robots;

ESP_LOGI(TAG, "🪆 ตุ๊กตา: %d ตัว", dolls);
ESP_LOGI(TAG, "🤖 หุ่นยนต์: %d ตัว", robots);
ESP_LOGI(TAG, "🎯 ของเล่นทั้งหมด: %d ชิ้น", total_toys);

📝 แบบฝึกหัดที่ 4: คำถามให้คิด
หากน้องอยากแจกของเล่นให้เพื่อน 10 คน คนละ 2 ชิ้น:

ต้องมีของเล่นกี่ชิ้น?
- 20 ชิ้น
ถ้ามี 15 ชิ้น จะขาดกี่ชิ้น?
- 5 ชิ้น

โค้ด
----------------------------------------
#include <stdio.h>
#include "esp_log.h"
#include "freertos/FreeRTOS.h"
#include "freertos/task.h"

static const char *TAG = "TOYS_MATH";

void app_main(void)
{
    ESP_LOGI(TAG, "🧸 เริ่มต้นโปรแกรมนับของเล่นของน้อง 🧸");
    ESP_LOGI(TAG, "=========================================");
    
    // ประกาศตัวแปรเก็บจำนวนของเล่น
    int toys_have = 15;        // ของเล่นที่น้องมีอยู่
    int toys_give_away = 7;   // ของเล่นที่แจกให้เพื่อน
    int toys_remaining;      // ของเล่นที่เหลือ
    int dolls = 5;        // ตุ๊กตา
    int robots = 2;       // หุ่นยนต์
    int total_toys = toys_have + dolls + robots;

    ESP_LOGI(TAG, "🪆 ตุ๊กตา: %d ตัว", dolls);
    ESP_LOGI(TAG, "🤖 หุ่นยนต์: %d ตัว", robots);
    ESP_LOGI(TAG, "🎯 ของเล่นทั้งหมด: %d ชิ้น", total_toys);
    ESP_LOGI(TAG, "");

    // ตรวจสอบว่าของเล่นพอแจกไหม

    if (toys_have >= toys_give_away) {
    ESP_LOGI(TAG, "✅ ของเล่นพอแจก");
    } else {
    ESP_LOGI(TAG, "❌ ของเล่นไม่พอ! ขาดไป %d ชิ้น", 
             toys_give_away - toys_have);
    }
    
    // แสดงข้อมูลเริ่มต้น
    ESP_LOGI(TAG, "📖 โจทย์:");
    ESP_LOGI(TAG, "   น้องมีของเล่น: %d ชิ้น", toys_have);
    ESP_LOGI(TAG, "   เอาไปแจกให้เพื่อน: %d ชิ้น", toys_give_away);
    ESP_LOGI(TAG, "   ❓ น้องเหลือของเล่นกี่ชิ้น?");
    ESP_LOGI(TAG, "");
    
    
    vTaskDelay(3000 / portTICK_PERIOD_MS);
    
    // ตรวจสอบก่อนคำนวณ
    if (toys_give_away > toys_have) {
        ESP_LOGW(TAG, "⚠️  คำเตือน: ของเล่นที่จะแจก (%d) มากกว่าที่มีอยู่ (%d)!", 
                 toys_give_away, toys_have);
        ESP_LOGI(TAG, "   น้องไม่สามารถแจกของเล่นได้มากกว่าที่มี");
        ESP_LOGI(TAG, "   จะแจกได้เฉพาะ %d ชิ้น (ทั้งหมดที่มี)", toys_have);
        toys_give_away = toys_have;
    }


    
    // คำนวณผลลัพธ์ (การลบ)
    toys_remaining = toys_have - toys_give_away;
    
    // แสดงขั้นตอนการคิด
    ESP_LOGI(TAG, "🧮 ขั้นตอนการคิด:");
    ESP_LOGI(TAG, "   ของเล่นที่มี - ของเล่นที่แจก");
    ESP_LOGI(TAG, "   = %d - %d", toys_have, toys_give_away);
    ESP_LOGI(TAG, "   = %d ชิ้น", toys_remaining);
    ESP_LOGI(TAG, "");
    
    // แสดงคำตอบ
    ESP_LOGI(TAG, "✅ คำตอบ:");
    ESP_LOGI(TAG, "   น้องเหลือของเล่น %d ชิ้น", toys_remaining);
    ESP_LOGI(TAG, "");
    
    // แสดงภาพประกอบ
    ESP_LOGI(TAG, "🎨 ภาพประกอบ:");
    ESP_LOGI(TAG, "   ของเล่นเดิม: 🧸🚗🎲🧩🎮🧸🚁🎯 (%d ชิ้น)", toys_have);
    ESP_LOGI(TAG, "   แจกให้เพื่อน: 🧸🚗🎲 (%d ชิ้น)", toys_give_away);
    ESP_LOGI(TAG, "   เหลืออยู่:   🧩🎮🧸🚁🎯 (%d ชิ้น)", toys_remaining);
    ESP_LOGI(TAG, "");
    
    // ตัวอย่างเพิ่มเติม
    ESP_LOGI(TAG, "💡 ตัวอย่างเพิ่มเติม:");
    
    // ตัวอย่างที่ 1
    int ex1_have = 15;
    int ex1_give = 7;
    int ex1_remain = ex1_have - ex1_give;
    ESP_LOGI(TAG, "   ถ้าน้องมีของเล่น %d ชิ้น และแจกไป %d ชิ้น", ex1_have, ex1_give);
    ESP_LOGI(TAG, "   จะเหลือ %d - %d = %d ชิ้น", ex1_have, ex1_give, ex1_remain);
    ESP_LOGI(TAG, "");
    
    // ตัวอย่างที่ 2 (กรณีพิเศษ)
    int ex2_have = 5;
    int ex2_give = 5;
    int ex2_remain = ex2_have - ex2_give;
    ESP_LOGI(TAG, "   ถ้าน้องมีของเล่น %d ชิ้น และแจกไปหมด %d ชิ้น", ex2_have, ex2_give);
    ESP_LOGI(TAG, "   จะเหลือ %d - %d = %d ชิ้น (ไม่เหลือเลย)", ex2_have, ex2_give, ex2_remain);
    ESP_LOGI(TAG, "");
    
    // เปรียบเทียบกับการบวก
    ESP_LOGI(TAG, "🔄 เปรียบเทียบกับการบวก:");
    ESP_LOGI(TAG, "   การบวก: เพิ่มจำนวน (เช่น ไข่ 4 + 2 = 6)");
    ESP_LOGI(TAG, "   การลบ: ลดจำนวน (เช่น ของเล่น 8 - 3 = 5)");
    ESP_LOGI(TAG, "   ข้อแตกต่าง: การลบต้องระวังไม่ให้ติดลบ!");
    ESP_LOGI(TAG, "");
    
    // กรณีที่อาจเกิดปัญหา
    ESP_LOGI(TAG, "⚠️  กรณีที่ต้องระวัง:");
    int danger_have = 3;
    int danger_give = 5;
    ESP_LOGI(TAG, "   ถ้าน้องมีของเล่น %d ชิ้น แต่จะแจก %d ชิ้น", danger_have, danger_give);
    ESP_LOGI(TAG, "   จะได้ %d - %d = %d (ผลลัพธ์เป็นลบ!)", 
             danger_have, danger_give, danger_have - danger_give);
    ESP_LOGI(TAG, "   ในชีวิตจริง: น้องไม่สามารถแจกของเล่นมากกว่าที่มีได้");
    ESP_LOGI(TAG, "");
    
    // สรุปการเรียนรู้
    ESP_LOGI(TAG, "📚 สิ่งที่เรียนรู้:");
    ESP_LOGI(TAG, "   1. การลบเลข (Subtraction): a - b = c");
    ESP_LOGI(TAG, "   2. การตรวจสอบเงื่อนไข (if statement)");
    ESP_LOGI(TAG, "   3. การจัดการกรณีพิเศษ (edge cases)");
    ESP_LOGI(TAG, "   4. ความแตกต่างระหว่างการบวกและการลบ");
    ESP_LOGI(TAG, "   5. การป้องกันผลลัพธ์ที่ไม่สมเหตุสมผล");
    ESP_LOGI(TAG, "");
    
    ESP_LOGI(TAG, "🎉 จบโปรแกรมนับของเล่นของน้อง!");
    ESP_LOGI(TAG, "📖 อ่านต่อในโปรเจคถัดไป: 03_multiplication_candies");
    
    vTaskDelay(2000 / portTICK_PERIOD_MS);
}



ผลลัพธ์
-----------------------------------------------
I (3820) boot: Loaded app from partition at offset 0x10000
I (3821) boot: Disabling RNG early entropy source...
I (3892) cpu_start: Multicore app
I (7347) cpu_start: Pro cpu start user code
I (7349) cpu_start: cpu freq: 160000000 Hz
I (7349) app_init: Application information:
I (7349) app_init: Project name:     subtraction_toys
I (7349) app_init: App version:      5f208b0-dirty
I (7350) app_init: Compile time:     Jul 23 2025 12:09:54
I (7350) app_init: ELF file SHA256:  94dfb4362...
I (7350) app_init: ESP-IDF:          v6.0-dev-1002-gbfe5caf58f
I (7351) efuse_init: Min chip rev:     v0.0
I (7351) efuse_init: Max chip rev:     v3.99
I (7351) efuse_init: Chip rev:         v3.0
I (7353) heap_init: Initializing. RAM available for dynamic allocation:
I (7354) heap_init: At 3FFAE6E0 len 00001920 (6 KiB): DRAM
I (7355) heap_init: At 3FFB2EA8 len 0002D158 (180 KiB): DRAM
I (7355) heap_init: At 3FFE0440 len 00003AE0 (14 KiB): D/IRAM
I (7355) heap_init: At 3FFE4350 len 0001BCB0 (111 KiB): D/IRAM
I (7355) heap_init: At 4008CFB8 len 00013048 (76 KiB): IRAM
I (7431) spi_flash: detected chip: winbond
I (7450) spi_flash: flash io: dio
I (7475) main_task: Started on CPU0
I (7485) main_task: Calling app_main()
I (7485) TOYS_MATH: 🧸 เริ่มต้นโปรแกรมนับของเล่นของน้อง 🧸
I (7485) TOYS_MATH: =========================================
I (7485) TOYS_MATH: 🪆 ตุ๊กตา: 5 ตัว
I (7485) TOYS_MATH: 🤖 หุ่นยนต์: 2 ตัว
I (7485) TOYS_MATH: 🎯 ของเล่นทั้งหมด: 22 ชิ้น
I (7485) TOYS_MATH:
I (7485) TOYS_MATH: ✅ ของเล่นพอแจก
I (7485) TOYS_MATH: 📖 โจทย์:
I (7495) TOYS_MATH:    น้องมีของเล่น: 15 ชิ้น
I (7495) TOYS_MATH:    เอาไปแจกให้เพื่อน: 7 ชิ้น
I (7495) TOYS_MATH:    ❓ น้องเหลือของเล่นกี่ชิ้น?
I (7495) TOYS_MATH:
I (10495) TOYS_MATH: 🧮 ขั้นตอนการคิด:
I (10495) TOYS_MATH:    ของเล่นที่มี - ของเล่นที่แจก
I (10495) TOYS_MATH:    = 15 - 7
I (10495) TOYS_MATH:    = 8 ชิ้น
I (10495) TOYS_MATH: 
I (10495) TOYS_MATH: ✅ คำตอบ:
I (10495) TOYS_MATH:    น้องเหลือของเล่น 8 ชิ้น
I (10495) TOYS_MATH: 
I (10495) TOYS_MATH: 🎨 ภาพประกอบ:
I (10495) TOYS_MATH:    ของเล่นเดิม: 🧸🚗🎲🧩🎮🧸🚁🎯 (15 ชิ้น)
I (10495) TOYS_MATH:    แจกให้เพื่อน: 🧸🚗🎲 (7 ชิ้น)
I (10495) TOYS_MATH:    เหลืออยู่:   🧩🎮🧸🚁🎯 (8 ชิ้น)
I (10495) TOYS_MATH:
I (10495) TOYS_MATH: 💡 ตัวอย่างเพิ่มเติม:
I (10495) TOYS_MATH:    ถ้าน้องมีของเล่น 15 ชิ้น และแจกไป 7 ชิ้น
I (10495) TOYS_MATH:    จะเหลือ 15 - 7 = 8 ชิ้น
I (10495) TOYS_MATH:
I (10495) TOYS_MATH:    ถ้าน้องมีของเล่น 5 ชิ้น และแจกไปหมด 5 ชิ้น
I (10495) TOYS_MATH:    จะเหลือ 5 - 5 = 0 ชิ้น (ไม่เหลือเลย)
I (10495) TOYS_MATH:
I (10495) TOYS_MATH: 🔄 เปรียบเทียบกับการบวก:
I (10495) TOYS_MATH:    การบวก: เพิ่มจำนวน (เช่น ไข่ 4 + 2 = 6)
I (10505) TOYS_MATH:    การลบ: ลดจำนวน (เช่น ของเล่น 8 - 3 = 5)
I (10505) TOYS_MATH:    ข้อแตกต่าง: การลบต้องระวังไม่ให้ติดลบ!
I (10505) TOYS_MATH: 
I (10505) TOYS_MATH: ⚠️  กรณีที่ต้องระวัง:
I (10505) TOYS_MATH:    ถ้าน้องมีของเล่น 3 ชิ้น แต่จะแจก 5 ชิ้น
I (10505) TOYS_MATH:    จะได้ 3 - 5 = -2 (ผลลัพธ์เป็นลบ!)
I (10505) TOYS_MATH:    ในชีวิตจริง: น้องไม่สามารถแจกของเล่นมากกว่าที่มีได้
I (10505) TOYS_MATH:
I (10505) TOYS_MATH: 📚 สิ่งที่เรียนรู้:
I (10505) TOYS_MATH:    1. การลบเลข (Subtraction): a - b = c
I (10505) TOYS_MATH:    2. การตรวจสอบเงื่อนไข (if statement)
I (10505) TOYS_MATH:    3. การจัดการกรณีพิเศษ (edge cases)
I (10505) TOYS_MATH:    4. ความแตกต่างระหว่างการบวกและการลบ
I (10505) TOYS_MATH:    5. การป้องกันผลลัพธ์ที่ไม่สมเหตุสมผล
I (10505) TOYS_MATH: 
I (10505) TOYS_MATH: 🎉 จบโปรแกรมนับของเล่นของน้อง!
I (10505) TOYS_MATH: 📖 อ่านต่อในโปรเจคถัดไป: 03_multiplication_candies
I (12505) main_task: Returned from app_main()
