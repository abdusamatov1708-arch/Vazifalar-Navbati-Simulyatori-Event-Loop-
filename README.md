# Vazifalar-Navbati-Simulyatori-Event-Loop-
// ==========================================
// 1-qism: Asosiy tartib (Sync, Microtask, Macrotask)
// ==========================================
/*
   Kutiladigan natija tartibi: 1, 4, 3, 2
   Tushuntirish:
   - Synchronous (Sinxron) kodlar birinchi bo'lib Call Stack da bajariladi: 1 va 4.
   - Microtask Queue (Promise) Macrotask Queue dan ustunlikka ega: shuning uchun 3 chiqadi.
   - Macrotask Queue (setTimeout) eng oxirida bajariladi: shuning uchun 2 chiqadi.
*/

console.log("--- 1-QISM: Asosiy tartib ---");
console.log("1"); // Sinxron

setTimeout(() => {
    console.log("2"); // Macrotask (setTimeout)
}, 0);

Promise.resolve().then(() => {
    console.log("3"); // Microtask (Promise)
});

console.log("4"); // Sinxron

// Biroz kutib turamiz (1-qism konsolga to'liq yozilishi uchun)
setTimeout(() => {
    console.log("\n");
}, 100);


// ==========================================
// 2-qism: Murakkab stsenariy (Promise zanjiri va queueMicrotask)
// ==========================================
/*
   Kutiladigan natija tartibi: A, B, E, C, D, F
   Tushuntirish:
   - A va B - sinxron tarzda darhol ishlaydi.
   - queueMicrotask() va Promise.then() Microtask navbatiga tushadi.
   - E sinxron bo'lgani uchun tez chiqadi (yoki Promise zanjirining boshlanishi).
   - Ichma-ich Promise .then() lari navbatma-navbat microtask navbatining oxiriga qo'shilib boradi (C, D).
   - setTimeout eng oxirida Macrotask navbatidan bajariladi (F).
*/

setTimeout(() => {
    console.log("--- 2-QISM: Murakkab stsenariy ---");
    
    console.log("A");

    setTimeout(() => {
        console.log("F"); // Macrotask
    }, 0);

    Promise.resolve()
        .then(() => {
            console.log("C"); // Microtask 1
            return Promise.resolve("Inner Promise");
        })
        .then(() => {
            console.log("D"); // Microtask 2 (zanjir davomi)
        });

    queueMicrotask(() => {
        console.log("E"); // Maxsus Microtask
    });

    console.log("B");
}, 200);


// ==========================================
// 3-qism: Async/await va Event Loop o'zaro ta'siri
// ==========================================
/*
   Tushuntirish:
   - async funksiya chaqirilganda, uning tanasi `await` gacha sinxron ishlaydi.
   - `await` dan keyingi qism avtomatik ravishda Microtask (Promise) sifatida navbatga qo'yiladi.
   - Parallel ishga tushirilgan async funksiyalar o'z navbatida Microtask va Macrotasklar bilan qanday navbatlashishini ko'ramiz.
*/

setTimeout(() => {
    console.log("\n--- 3-QISM: Async/await va Event Loop ---");

    async function asyncTest() {
        console.log("1. Async boshlandi"); // Sinxron (Call Stack)
        
        await new Promise(resolve => setTimeout(resolve, 0)); // Macrotask kutilyapti
        
        console.log("2. Await dan keyin (Microtask)"); // Macrotask ichidagi microtask
    }

    console.log("3. Funksiyadan tashqari (Boshlanish)");
    asyncTest();
    console.log("4. Funksiyadan tashqari (Tugash)");

}, 400);
