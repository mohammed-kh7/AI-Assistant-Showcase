<div align="center">

# 🤖 AI Assistant Bot
### بوت تيليجرام ذكي بواجهتين (عميل ومالك) مدعوم بتقنية RAG وأتمتة التقاط العملاء

[![n8n](https://img.shields.io/badge/n8n-Workflow%20Automation-EA4B71?style=for-the-badge&logo=n8n&logoColor=white)](https://n8n.io/)
[![Google Gemini](https://img.shields.io/badge/Google%20Gemini-3.5%20Flash--Lite-4285F4?style=for-the-badge&logo=google&logoColor=white)](https://ai.google.dev/)
[![Qdrant](https://img.shields.io/badge/Qdrant-Vector%20Search%20(topK=15)-DC2626?style=for-the-badge&logo=qdrant&logoColor=white)](https://qdrant.tech/)
[![Cohere](https://img.shields.io/badge/Cohere-Rerank%20v3.0%20(topK=5)-39594C?style=for-the-badge)](https://cohere.com/)
[![Telegram](https://img.shields.io/badge/Telegram-Dual%20Interface%20Bot-24A1DE?style=for-the-badge&logo=telegram&logoColor=white)](https://telegram.org/)

<p align="center">
  <b>مشروع التخرج المعتمد · تدريب الذكاء الاصطناعي والأتمتة</b><br>
  جامعة النجاح الوطنية · برنامج الأمم المتحدة الإنمائي (UNDP) · مركز شؤون المرأة - غزة
</p>

**إعداد وتطوير:** م. محمد خضير &nbsp;|&nbsp; **إشراف:** م. مهند هندي &nbsp;|&nbsp; **تاريخ الاعتماد:** 11 أكتوبر 2026

---

</div>

## 📊 إحصائيات وأرقام المنظومة (Key Metrics)

<div align="center">

| 👥 الواجهات | 🔄 التدفقات البرمجية | 🧩 العقد المؤتمتة | 🔀 مسارات التوجيه | ⚡ زمن الاستجابة |
| :---: | :---: | :---: | :---: | :---: |
| **واجهتان (عميل ومالك)** | **3 فلوهات مترابطة** | **93 عقدة في n8n** | **9 مسارات في Router** | **فوري على مدار 24/7** |

</div>

---

## 🎯 1. المشكلة والحل (Problem & Solution)

| قبل الأتمتة (Before) | بعد تطبيق النظام (After) |
| :--- | :--- |
| • العميل المحتمل يسأل نفس الأسئلة المتكررة حول الهوية والخدمات. | • **واجهتان مستقلتان:** واجهة عميل تفاعلية وواجهة مالك إدارية بنفس البوت. |
| • الرد اليدوي غير الفوري مما يضيع فرص عمل وعملاء مؤكدين. | • **رد ذكي فوري 24/7** يستند بدقة للمستندات دون أي اختلاق (Zero Hallucination). |
| • الطلبات تُكتب يدوياً في المحادثات فتضيع أو تتأخر متابعتها. | • **التقاط آلي للبيانات (Lead Capture):** حفظ فوري بالـ Sheets مع إشعار Gmail وتيليجرام. |
| • تحديث بيانات السيرة أو المعرفة يتطلب تدخلاً تقنياً معقداً. | • **إدارة معرفة لحظية:** المالك يرفع المستندات من تيليجرام وتُفهرس فوراً في Qdrant. |

---

## 🏛️ 2. البنية المعمارية للتدفقات الثلاثة (System Architecture)

تم تقسيم المنظومة هندسياً إلى 3 فلوهات فرعية داخل n8n لضمان استقرار الأداء وعزل الأخطاء:

```mermaid
graph TD
    Client([العميل المحتمل]) <-->|رسائل وأزرار| TG[Telegram Bot API]
    Owner([صاحب العمل / المالك]) <-->|لوحة التحكم والملفات| TG

    subgraph n8n_Core [منظومة n8n للأتمتة الذكية]
        TG <--> WF1["① AI Assistant Bot (36 Nodes)<br>• Router بـ 9 مخارج<br>• إدارة تدفق الطلبات<br>• وكيل الذكاء الاصطناعي"]
        
        WF1 <-->|استدعاء لوحة التحكم| WF2["② Knowledge Manager (29 Nodes)<br>• لوحة تحكم المالك الحصرية<br>• رفع الملفات إلى Drive<br>• حذف المتجهات والسجلات"]
        
        WF2 -->|تشغيل الفهرسة| WF3["③ Qdrant Database (28 Nodes)<br>• اكتشاف الملفات الجديدة والمعدلة<br>• استخراج نصوص PDF/DOCX/MD<br>• Embeddings & Qdrant Upsert"]
    end

    WF1 --> GSheets[(Google Sheets)]
    WF1 --> Gmail[Gmail Alerts]
    WF3 <--> GDrive[(Google Drive)]
    WF3 <--> Qdrant[(Qdrant Vector DB)]
    WF1 <--> Gemini[Google Gemini LLM]
    WF1 <--> Cohere[Cohere Rerank v3.0]
