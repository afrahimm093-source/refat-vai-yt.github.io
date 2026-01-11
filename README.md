<!DOCTYPE html>
<html lang="bn">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>RT Official AI - Owner Refat</title>
    <style>
        body { font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif; background-color: #f4f4f9; display: flex; justify-content: center; align-items: center; height: 100vh; margin: 0; }
        .chat-container { width: 400px; background: white; border-radius: 15px; box-shadow: 0 5px 15px rgba(0,0,0,0.2); overflow: hidden; display: flex; flex-direction: column; }
        #chat-header { background: #007bff; color: white; padding: 15px; text-align: center; font-weight: bold; }
        #chat-box { height: 400px; overflow-y: auto; padding: 15px; display: flex; flex-direction: column; gap: 10px; }
        .message { padding: 8px 12px; border-radius: 10px; max-width: 80%; font-size: 14px; }
        .user { align-self: flex-end; background: #007bff; color: white; }
        .ai { align-self: flex-start; background: #e9ecef; color: #333; }
        .input-area { display: flex; padding: 10px; border-top: 1px solid #ddd; }
        input { flex: 1; border: 1px solid #ccc; padding: 10px; border-radius: 5px; outline: none; }
        button { background: #007bff; color: white; border: none; padding: 10px 15px; margin-left: 5px; border-radius: 5px; cursor: pointer; }
    </style>
</head>
<body>

<div class="chat-container">
    <div id="chat-header">RT Official AI</div>
    <div id="chat-box"></div>
    <div class="input-area">
        <input type="text" id="user-input" placeholder="কিছু জিজ্ঞাসা করুন...">
        <button onclick="sendMessage()">পাঠান</button>
    </div>
</div>

<script>
    const API_KEY = "YOUR_GEMINI_API_KEY"; // এখানে আপনার API Key বসান
    const API_URL = `https://generativelanguage.googleapis.com/v1beta/models/gemini-1.5-flash:generateContent?key=${API_KEY}`;

    // এআই-এর জন্য আপনার দেওয়া কাস্টম তথ্য (System Instruction)
    const systemPrompt = "আপনার নাম 'RT Official AI'। আপনার মালিকের নাম 'রিফাত' (Refat)। আপনাকে 'আরটি অফিসিয়াল সাইট' থেকে বানানো হয়েছে। এই প্রশ্নগুলোর উত্তর সবসময় এভাবে দেবেন।";

    async function sendMessage() {
        const inputField = document.getElementById("user-input");
        const chatBox = document.getElementById("chat-box");
        const userText = inputField.value.trim();

        if (!userText) return;

        // ইউজারের মেসেজ স্ক্রিনে দেখানো
        chatBox.innerHTML += `<div class="message user">${userText}</div>`;
        inputField.value = "";

        try {
            const response = await fetch(API_URL, {
                method: "POST",
                headers: { "Content-Type": "application/json" },
                body: JSON.stringify({
                    contents: [{
                        parts: [{ text: systemPrompt + " User asked: " + userText }]
                    }]
                })
            });

            const data = await response.json();
            const aiResponse = data.candidates[0].content.parts[0].text;

            // এআই-এর উত্তর স্ক্রিনে দেখানো
            chatBox.innerHTML += `<div class="message ai">${aiResponse}</div>`;
            chatBox.scrollTop = chatBox.scrollHeight;
        } catch (error) {
            chatBox.innerHTML += `<div class="message ai">দুঃখিত, কানেকশনে সমস্যা হচ্ছে। API Key চেক করুন।</div>`;
        }
    }
</script>

</body>
</html>
