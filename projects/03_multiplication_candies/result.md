📝 แบบฝึกหัดที่ 1: เปลี่ยนจำนวนถุงลูกอม
// หาบรรทัดนี้ในโค้ด:
int candy_bags = 5;         // จำนวนถุง
int candies_per_bag = 6;    // ลูกอมต่อถุง

// ลองเปลี่ยนเป็น:
int candy_bags = 7;         // เพิ่มเป็น 7 ถุง
int candies_per_bag = 8;    // ลูกอมถุงละ 8 เม็ด
📝 แบบฝึกหัดที่ 2: เพิ่มลูกอมหลายรส
เพิ่มลูกอมหลายรส:

int strawberry_bags = 3;    // ถุงรสสตรอเบอร์รี่
int orange_bags = 2;        // ถุงรสส้ม
int grape_bags = 4;         // ถุงรสองุ่น

int total_bags = strawberry_bags + orange_bags + grape_bags;
int total_candies = total_bags * candies_per_bag;

ESP_LOGI(TAG, "🍓 สตรอเบอร์รี่: %d ถุง = %d เม็ด", 
         strawberry_bags, strawberry_bags * candies_per_bag);
ESP_LOGI(TAG, "🍊 รสส้ม: %d ถุง = %d เม็ด", 
         orange_bags, orange_bags * candies_per_bag);
ESP_LOGI(TAG, "🍇 รสองุ่น: %d ถุง = %d เม็ด", 
         grape_bags, grape_bags * candies_per_bag);
📝 แบบฝึกหัดที่ 3: สร้างตารางสูตรคูณ
เพิ่มการแสดงตารางสูตรคูณ:

ESP_LOGI(TAG, "📊 ตารางสูตรคูณของ %d:", candies_per_bag);
for (int i = 1; i <= 10; i++) {
    ESP_LOGI(TAG, "   %d x %d = %d", i, candies_per_bag, i * candies_per_bag);
}
📝 แบบฝึกหัดที่ 4: แจกลูกอมให้เพื่อน
คำนวณการแจกลูกอม:

int friends = 12;           // จำนวนเพื่อน
int candies_per_friend = total_candies / friends;  // ลูกอมต่อคน
int remaining_candies = total_candies % friends;   // ลูกอมที่เหลือ

ESP_LOGI(TAG, "👥 แจกให้เพื่อน %d คน:", friends);
ESP_LOGI(TAG, "   คนละ %d เม็ด", candies_per_friend);
ESP_LOGI(TAG, "   เหลือ %d เม็ด", remaining_candies);


โค้ด
------------------------------------------
#include <stdio.h>
#include "esp_log.h"
#include "freertos/FreeRTOS.h"
#include "freertos/task.h"

static const char *TAG = "CANDIES_MATH";

void app_main(void)
{
    ESP_LOGI(TAG, "🍬 เริ่มต้นโปรแกรมนับลูกอมในถุง 🍬");
    ESP_LOGI(TAG, "====================================");
    
    // ประกาศตัวแปรเก็บจำนวนลูกอม
    int number_of_bags = 7;        // จำนวนถุง
    int candies_per_bag = 8;       // ลูกอมต่อถุง
    int total_candies;             // ลูกอมทั้งหมด

    // 🎨 แสดงลูกอมหลายรสชาติ
    int strawberry_bags = 3;    // ถุงรสสตรอเบอร์รี่
    int orange_bags = 2;        // ถุงรสส้ม
    int grape_bags = 4;         // ถุงรสองุ่น

    int total_bags = strawberry_bags + orange_bags + grape_bags;
    int flavored_total_candies = total_bags * candies_per_bag;

    ESP_LOGI(TAG, "🍬 ลูกอมแยกตามรสชาติ:");
    ESP_LOGI(TAG, "   🍓 สตรอเบอร์รี่: %d ถุง × %d = %d เม็ด", 
             strawberry_bags, candies_per_bag, strawberry_bags * candies_per_bag);
    ESP_LOGI(TAG, "   🍊 ส้ม:           %d ถุง × %d = %d เม็ด", 
             orange_bags, candies_per_bag, orange_bags * candies_per_bag);
    ESP_LOGI(TAG, "   🍇 องุ่น:         %d ถุง × %d = %d เม็ด", 
             grape_bags, candies_per_bag, grape_bags * candies_per_bag);
    ESP_LOGI(TAG, "   📦 รวมทั้งหมด:   %d ถุง = %d เม็ด", total_bags, flavored_total_candies);
    ESP_LOGI(TAG, "");
    
    // แสดงข้อมูลเริ่มต้น
    ESP_LOGI(TAG, "📖 โจทย์:");
    ESP_LOGI(TAG, "   มีถุงลูกอม: %d ถุง", number_of_bags);
    ESP_LOGI(TAG, "   ถุงละ: %d เม็ด", candies_per_bag);
    ESP_LOGI(TAG, "   ❓ มีลูกอมทั้งหมดกี่เม็ด?");
    ESP_LOGI(TAG, "");
    
    vTaskDelay(3000 / portTICK_PERIOD_MS);
    
    // คำนวณผลรวม (การคูณ)
    total_candies = number_of_bags * candies_per_bag;
    
    // แสดงขั้นตอนการคิด
    ESP_LOGI(TAG, "🧮 ขั้นตอนการคิด:");
    ESP_LOGI(TAG, "   จำนวนถุง × ลูกอมต่อถุง");
    ESP_LOGI(TAG, "   = %d × %d", number_of_bags, candies_per_bag);
    ESP_LOGI(TAG, "   = %d เม็ด", total_candies);
    ESP_LOGI(TAG, "");

    int friends = 12;           // จำนวนเพื่อน
    int candies_per_friend = total_candies / friends;  // ลูกอมต่อคน
    int remaining_candies = total_candies % friends;   // ลูกอมที่เหลือ

    ESP_LOGI(TAG, "👥 แบ่งลูกอมให้เพื่อน %d คน:", friends);
    ESP_LOGI(TAG, "   คนละ %d เม็ด", candies_per_friend);
    ESP_LOGI(TAG, "   🍭 เหลือ %d เม็ด (ยังไม่รู้จะแจกใคร)", remaining_candies);
    ESP_LOGI(TAG, "");
    
    // แสดงคำตอบ
    ESP_LOGI(TAG, "✅ คำตอบ:");
    ESP_LOGI(TAG, "   มีลูกอมทั้งหมด %d เม็ด", total_candies);
    ESP_LOGI(TAG, "");
    
    // แสดงภาพประกอบ
    ESP_LOGI(TAG, "🎨 ภาพประกอบ:");
    ESP_LOGI(TAG, "   ถุงที่ 1: 🍬🍬🍬🍬🍬🍬 (%d เม็ด)", candies_per_bag);
    ESP_LOGI(TAG, "   ถุงที่ 2: 🍬🍬🍬🍬🍬🍬 (%d เม็ด)", candies_per_bag);
    ESP_LOGI(TAG, "   ถุงที่ 3: 🍬🍬🍬🍬🍬🍬 (%d เม็ด)", candies_per_bag);
    ESP_LOGI(TAG, "   ถุงที่ 4: 🍬🍬🍬🍬🍬🍬 (%d เม็ด)", candies_per_bag);
    ESP_LOGI(TAG, "   ถุงที่ 5: 🍬🍬🍬🍬🍬🍬 (%d เม็ด)", candies_per_bag);
    ESP_LOGI(TAG, "   รวม:     %d เม็ด", total_candies);
    ESP_LOGI(TAG, "");
    
    // เปรียบเทียบกับการบวกซ้ำๆ
    ESP_LOGI(TAG, "🔄 เปรียบเทียบกับการบวกซ้ำๆ:");
    ESP_LOGI(TAG, "   การคูณ: %d × %d = %d", number_of_bags, candies_per_bag, total_candies);
    
    // แสดงการบวกซ้ำๆ
    ESP_LOGI(TAG, "   การบวกซ้ำๆ: ");
    int sum_check = 0;
    for (int i = 1; i <= number_of_bags; i++) {
        sum_check += candies_per_bag;
        if (i == 1) {
            ESP_LOGI(TAG, "                  %d", candies_per_bag);
        } else if (i < number_of_bags) {
            ESP_LOGI(TAG, "                + %d", candies_per_bag);
        } else {
            ESP_LOGI(TAG, "                + %d = %d", candies_per_bag, sum_check);
        }
    }
    ESP_LOGI(TAG, "   ผลลัพธ์เหมือนกัน! การคูณคือการบวกซ้ำๆ");
    ESP_LOGI(TAG, "");
    
    // ตารางสูตรคูณ
    ESP_LOGI(TAG, "📊 ตารางสูตรคูณ %d:", candies_per_bag);
    for (int i = 1; i <= 10; i++) {
        ESP_LOGI(TAG, "   %d × %d = %d", i, candies_per_bag, i * candies_per_bag);
        vTaskDelay(300 / portTICK_PERIOD_MS); // หน่วงเล็กน้อยเพื่อให้อ่านง่าย
    }
    ESP_LOGI(TAG, "");

    ESP_LOGI(TAG, "📊 ตารางสูตรคูณของ %d:", candies_per_bag);
    for (int i = 1; i <= 10; i++) {
    ESP_LOGI(TAG, "   %d x %d = %d", i, candies_per_bag, i * candies_per_bag);
    }
    
    // ตัวอย่างเพิ่มเติม
    ESP_LOGI(TAG, "💡 ตัวอย่างเพิ่มเติม:");
    
    // ตัวอย่างที่ 1
    int ex1_bags = 3;
    int ex1_candies = 8;
    int ex1_total = ex1_bags * ex1_candies;
    ESP_LOGI(TAG, "   ถ้ามีถุงลูกอม %d ถุง ถุงละ %d เม็ด", ex1_bags, ex1_candies);
    ESP_LOGI(TAG, "   จะได้ลูกอม %d × %d = %d เม็ด", ex1_bags, ex1_candies, ex1_total);
    ESP_LOGI(TAG, "");
    
    // ตัวอย่างที่ 2
    int ex2_bags = 7;
    int ex2_candies = 4;
    int ex2_total = ex2_bags * ex2_candies;
    ESP_LOGI(TAG, "   ถ้ามีถุงลูกอม %d ถุง ถุงละ %d เม็ด", ex2_bags, ex2_candies);
    ESP_LOGI(TAG, "   จะได้ลูกอม %d × %d = %d เม็ด", ex2_bags, ex2_candies, ex2_total);
    ESP_LOGI(TAG, "");
    
    // เปรียบเทียบกับการดำเนินการก่อนหน้า
    ESP_LOGI(TAG, "🔄 เปรียบเทียบการดำเนินการ:");
    ESP_LOGI(TAG, "   การบวก (+): เพิ่มจำนวน (เช่น ไข่ 4 + 2 = 6)");
    ESP_LOGI(TAG, "   การลบ (-): ลดจำนวน (เช่น ของเล่น 8 - 3 = 5)");
    ESP_LOGI(TAG, "   การคูณ (×): บวกซ้ำๆ (เช่น ลูกอม 5 × 6 = 30)");
    ESP_LOGI(TAG, "");
    
    // แนวคิดขั้นสูง
    ESP_LOGI(TAG, "🎓 แนวคิดขั้นสูง:");
    ESP_LOGI(TAG, "   1. การคูณมีคุณสมบัติการสับเปลี่ยน:");
    ESP_LOGI(TAG, "      %d × %d = %d × %d = %d", 
             number_of_bags, candies_per_bag, candies_per_bag, number_of_bags, total_candies);
    ESP_LOGI(TAG, "   2. การคูณด้วย 0 จะได้ 0 เสมอ:");
    ESP_LOGI(TAG, "      %d × 0 = 0 (ไม่มีถุงลูกอม)", number_of_bags);
    ESP_LOGI(TAG, "   3. การคูณด้วย 1 จะได้ตัวเลขเดิม:");
    ESP_LOGI(TAG, "      %d × 1 = %d (มีถุงเดียว)", candies_per_bag, candies_per_bag);
    ESP_LOGI(TAG, "");
    
    // สรุปการเรียนรู้
    ESP_LOGI(TAG, "📚 สิ่งที่เรียนรู้:");
    ESP_LOGI(TAG, "   1. การคูณเลข (Multiplication): a × b = c");
    ESP_LOGI(TAG, "   2. การใช้ for loop สำหรับการทำซ้ำ");
    ESP_LOGI(TAG, "   3. ความสัมพันธ์ระหว่างการคูณและการบวกซ้ำๆ");
    ESP_LOGI(TAG, "   4. คุณสมบัติพิเศษของการคูณ");
    ESP_LOGI(TAG, "   5. การแสดงผลแบบตาราง");
    ESP_LOGI(TAG, "");
    
    ESP_LOGI(TAG, "🎉 จบโปรแกรมนับลูกอมในถุง!");
    ESP_LOGI(TAG, "📖 อ่านต่อในโปรเจคถัดไป: 04_division_cookies");
    
    vTaskDelay(2000 / portTICK_PERIOD_MS);
}


ผลลัพธ์
----------------------------------------
I (13237) CANDIES_MATH: 🧮 ขั้นตอนการคิด:
I (13237) CANDIES_MATH:    จำนวนถุง × ลูกอมต่อถุง
I (13237) CANDIES_MATH:    = 7 × 8
I (13237) CANDIES_MATH:    = 56 เม็ด
I (13237) CANDIES_MATH:
I (13237) CANDIES_MATH: 👥 แบ่งลูกอมให้เพื่อน 12 คน:
I (13237) CANDIES_MATH:    คนละ 4 เม็ด
I (13237) CANDIES_MATH:    🍭 เหลือ 8 เม็ด (ยังไม่รู้จะแจกใคร)
I (13237) CANDIES_MATH:
I (13237) CANDIES_MATH: ✅ คำตอบ:
I (13237) CANDIES_MATH:    มีลูกอมทั้งหมด 56 เม็ด
I (13237) CANDIES_MATH:
I (13237) CANDIES_MATH: 🎨 ภาพประกอบ:
I (13237) CANDIES_MATH:    ถุงที่ 1: 🍬🍬🍬🍬🍬🍬 (8 เม็ด)
I (13237) CANDIES_MATH:    ถุงที่ 2: 🍬🍬🍬🍬🍬🍬 (8 เม็ด)
I (13237) CANDIES_MATH:    ถุงที่ 3: 🍬🍬🍬🍬🍬🍬 (8 เม็ด)
I (13237) CANDIES_MATH:    ถุงที่ 4: 🍬🍬🍬🍬🍬🍬 (8 เม็ด)
I (13237) CANDIES_MATH:    ถุงที่ 5: 🍬🍬🍬🍬🍬🍬 (8 เม็ด)
I (13237) CANDIES_MATH:    รวม:     56 เม็ด
I (13237) CANDIES_MATH:
I (13237) CANDIES_MATH: 🔄 เปรียบเทียบกับการบวกซ้ำๆ:
I (13237) CANDIES_MATH:    การคูณ: 7 × 8 = 56
I (13237) CANDIES_MATH:    การบวกซ้ำๆ: 
I (13237) CANDIES_MATH:                   8
I (13237) CANDIES_MATH:                 + 8
I (13237) CANDIES_MATH:                 + 8
I (13237) CANDIES_MATH:                 + 8
I (13237) CANDIES_MATH:                 + 8
I (13237) CANDIES_MATH:                 + 8
I (13247) CANDIES_MATH:                 + 8 = 56
I (13247) CANDIES_MATH:    ผลลัพธ์เหมือนกัน! การคูณคือการบวกซ้ำๆ
I (13247) CANDIES_MATH:
I (13247) CANDIES_MATH: 📊 ตารางสูตรคูณ 8:
I (13247) CANDIES_MATH:    1 × 8 = 8
I (13547) CANDIES_MATH:    2 × 8 = 16
I (13847) CANDIES_MATH:    3 × 8 = 24
I (14147) CANDIES_MATH:    4 × 8 = 32
I (14447) CANDIES_MATH:    5 × 8 = 40
I (14747) CANDIES_MATH:    6 × 8 = 48
I (15047) CANDIES_MATH:    7 × 8 = 56
I (15347) CANDIES_MATH:    8 × 8 = 64
I (15647) CANDIES_MATH:    9 × 8 = 72
I (15947) CANDIES_MATH:    10 × 8 = 80
I (16247) CANDIES_MATH: 
I (16247) CANDIES_MATH: 📊 ตารางสูตรคูณของ 8:
I (16247) CANDIES_MATH:    1 x 8 = 8
I (16247) CANDIES_MATH:    2 x 8 = 16
I (16247) CANDIES_MATH:    3 x 8 = 24
I (16247) CANDIES_MATH:    4 x 8 = 32
I (16247) CANDIES_MATH:    5 x 8 = 40
I (16247) CANDIES_MATH:    6 x 8 = 48
I (16247) CANDIES_MATH:    7 x 8 = 56
I (16247) CANDIES_MATH:    8 x 8 = 64
I (16247) CANDIES_MATH:    9 x 8 = 72
I (16247) CANDIES_MATH:    10 x 8 = 80
I (16247) CANDIES_MATH: 💡 ตัวอย่างเพิ่มเติม:
I (16247) CANDIES_MATH:    ถ้ามีถุงลูกอม 3 ถุง ถุงละ 8 เม็ด
I (16247) CANDIES_MATH:    จะได้ลูกอม 3 × 8 = 24 เม็ด
I (16247) CANDIES_MATH: 
I (16247) CANDIES_MATH:    ถ้ามีถุงลูกอม 7 ถุง ถุงละ 4 เม็ด
I (16247) CANDIES_MATH:    จะได้ลูกอม 7 × 4 = 28 เม็ด
I (16247) CANDIES_MATH:
I (16257) CANDIES_MATH: 🔄 เปรียบเทียบการดำเนินการ:
I (16257) CANDIES_MATH:    การบวก (+): เพิ่มจำนวน (เช่น ไข่ 4 + 2 = 6)
I (16257) CANDIES_MATH:    การลบ (-): ลดจำนวน (เช่น ของเล่น 8 - 3 = 5)
I (16257) CANDIES_MATH:    การคูณ (×): บวกซ้ำๆ (เช่น ลูกอม 5 × 6 = 30)
I (16257) CANDIES_MATH:
I (16257) CANDIES_MATH: 🎓 แนวคิดขั้นสูง:
I (16257) CANDIES_MATH:    1. การคูณมีคุณสมบัติการสับเปลี่ยน:
I (16257) CANDIES_MATH:       7 × 8 = 8 × 7 = 56
I (16257) CANDIES_MATH:    2. การคูณด้วย 0 จะได้ 0 เสมอ:
I (16257) CANDIES_MATH:       7 × 0 = 0 (ไม่มีถุงลูกอม)
I (16257) CANDIES_MATH:    3. การคูณด้วย 1 จะได้ตัวเลขเดิม:
I (16257) CANDIES_MATH:       8 × 1 = 8 (มีถุงเดียว)
I (16257) CANDIES_MATH: 
I (16257) CANDIES_MATH: 📚 สิ่งที่เรียนรู้:
I (16257) CANDIES_MATH:    1. การคูณเลข (Multiplication): a × b = c
I (16257) CANDIES_MATH:    2. การใช้ for loop สำหรับการทำซ้ำ
I (16257) CANDIES_MATH:    3. ความสัมพันธ์ระหว่างการคูณและการบวกซ้ำๆ
I (16257) CANDIES_MATH:    4. คุณสมบัติพิเศษของการคูณ
I (16257) CANDIES_MATH:    5. การแสดงผลแบบตาราง
I (16257) CANDIES_MATH:
I (16257) CANDIES_MATH: 🎉 จบโปรแกรมนับลูกอมในถุง!
I (16257) CANDIES_MATH: 📖 อ่านต่อในโปรเจคถัดไป: 04_division_cookies
I (18257) main_task: Returned from app_main()
