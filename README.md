<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>استبيان المعتقدات الشخصية</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        body {
            background-color: #F3F4F6;
            font-family: 'Inter', sans-serif;
        }
        .container {
            max-width: 800px;
        }
        .page {
            display: none;
        }
        .page.active {
            display: block;
        }
        .form-input, .form-select {
            border-radius: 0.5rem;
            padding: 0.75rem;
            border: 1px solid #D1D5DB;
            background-color: #FFFFFF;
            width: 100%;
        }
        .card {
            background-color: #FFFFFF;
            border-radius: 1rem;
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
            padding: 2rem;
        }
        .radio-label {
            display: flex;
            align-items: center;
            justify-content: center;
            height: 100%;
            border-radius: 0.5rem;
            border: 2px solid #D1D5DB;
            cursor: pointer;
            transition: all 0.3s ease;
            text-align: center;
            padding: 0.5rem;
        }
        .radio-label:hover {
            background-color: #E5E7EB;
        }
        input[type="radio"]:checked + .radio-label {
            background-color: #10B981;
            border-color: #10B981;
            color: #FFFFFF;
            box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);
        }
        .progress-bar {
            height: 0.75rem;
            background-color: #E5E7EB;
            border-radius: 9999px;
        }
        .progress-fill {
            height: 100%;
            background-color: #10B981;
            border-radius: 9999px;
            transition: width 0.4s ease;
        }
        .summary-box {
            background-color: #F9FAFB;
            border-radius: 0.75rem;
            padding: 1.5rem;
            margin-top: 1rem;
        }
        .summary-item {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 0.5rem 0;
            border-bottom: 1px solid #E5E7EB;
        }
        .bar-chart {
            background-color: #F9FAFB;
            padding: 1rem;
            border-radius: 0.75rem;
        }
        .bar {
            height: 25px;
            margin-bottom: 8px;
            background-color: #10B981;
            border-radius: 4px;
            transition: width 0.5s ease-in-out;
        }
        .tooltip {
            position: relative;
            display: inline-block;
        }
        .tooltip .tooltiptext {
            visibility: hidden;
            width: 150px;
            background-color: #333;
            color: #fff;
            text-align: center;
            border-radius: 6px;
            padding: 5px;
            position: absolute;
            z-index: 1;
            bottom: 125%;
            left: 50%;
            margin-left: -75px;
            opacity: 0;
            transition: opacity 0.3s;
        }
        .tooltip:hover .tooltiptext {
            visibility: visible;
            opacity: 1;
        }
        @media print {
            body * {
                visibility: hidden;
            }
            #results-page, #results-page * {
                visibility: visible;
            }
            #results-page {
                position: absolute;
                left: 0;
                top: 0;
                width: 100%;
            }
        }
    </style>
</head>
<body class="bg-gray-100 flex items-center justify-center min-h-screen p-4">
    <div id="survey-container" class="container mx-auto p-8 card">

        <!-- Initial Page -->
        <div id="page-0" class="page active text-center">
            <h1 class="text-3xl font-bold text-gray-800 mb-4">استبيان المعتقدات الشخصية، نموذج صغير</h1>
            <p class="text-xs text-gray-500 mb-4">
                **ملاحظة:** هذا النموذج هو النموذج المصغر من الاستبيان الكبير للمعتقدات الشخصية والمعتمد من <a href="https://beckinstitute.org/wp-content/uploads/2021/06/PBQ-Full-Documents-1.pdf#page=1.11" target="_blank" class="text-blue-500 hover:underline">معهد بيك</a>.
            </p>
            <p class="text-gray-600 mb-8">
                من فضلك اقرأ البيانات المذكورة أدناه وقم بتحديد تصنيف لمدى اعتناقك لكل منها. وحاول أن تحكم على شعورك تجاه كل من هذه
                البيانات أغلب الوقت. ولا تترك أيا منها دون علامة.
            </p>
            <div class="space-y-4 text-right mb-8">
                <div>
                    <label for="name" class="block text-sm font-medium text-gray-700 mb-1">الاسم</label>
                    <input type="text" id="name" class="form-input" required>
                </div>
                <div>
                    <label for="age" class="block text-sm font-medium text-gray-700 mb-1">العمر (اختياري)</label>
                    <input type="number" id="age" class="form-input" min="1" max="120">
                </div>
                <div>
                    <label for="date" class="block text-sm font-medium text-gray-700 mb-1">التاريخ</label>
                    <input type="date" id="date" class="form-input" required>
                </div>
            </div>
            <button onclick="nextPage()" class="w-full px-6 py-3 bg-teal-500 text-white font-bold rounded-lg hover:bg-teal-600 focus:outline-none focus:ring-2 focus:ring-teal-500 transition-colors">
                ابدأ الاستبيان
            </button>
        </div>

        <!-- Survey Pages -->
        <div id="survey-content"></div>

        <!-- Navigation Buttons -->
        <div id="navigation-buttons" class="mt-8 flex justify-between">
            <button id="prev-btn" onclick="prevPage()" class="px-6 py-3 bg-gray-300 text-gray-700 font-bold rounded-lg hover:bg-gray-400 transition-colors" style="display: none;">
                السابق
            </button>
            <button id="next-btn" onclick="nextPage()" class="px-6 py-3 bg-teal-500 text-white font-bold rounded-lg hover:bg-teal-600 transition-colors" style="display: none;">
                التالي
            </button>
            <button id="submit-btn" onclick="showResults()" class="w-full px-6 py-3 bg-green-500 text-white font-bold rounded-lg hover:bg-green-600 transition-colors" style="display: none;">
                عرض النتائج
            </button>
        </div>

        <!-- Results Page -->
        <div id="results-page" class="page text-center">
            <h2 class="text-3xl font-bold text-gray-800 mb-6">نتائج الاستبيان</h2>
            <div id="results-summary" class="mb-8 text-right"></div>
            
            <h3 class="text-xl font-semibold text-gray-700 mb-4">الدرجات حسب أنماط الشخصية</h3>
            <div id="bar-chart" class="bar-chart"></div>

            <h3 class="text-xl font-semibold text-gray-700 mt-8 mb-4">ملخص الأنماط</h3>
            <div id="patterns-summary" class="space-y-4 text-right"></div>
            
            <div class="mt-8 space-y-4">
                <button onclick="generateGeminiSummary()" class="w-full px-6 py-3 bg-fuchsia-500 text-white font-bold rounded-lg hover:bg-fuchsia-600 transition-colors">
                    ✨ احصل على ملخص من Gemini
                </button>
                <div id="gemini-summary-container" class="summary-box" style="display: none;">
                    <div id="gemini-summary-text" class="text-sm text-gray-700"></div>
                </div>
                <button onclick="generateBehavioralAdvice()" class="w-full px-6 py-3 bg-purple-500 text-white font-bold rounded-lg hover:bg-purple-600 transition-colors">
                    ✨ احصل على نصائح سلوكية من Gemini
                </button>
                <div id="gemini-advice-container" class="summary-box" style="display: none;">
                    <div id="gemini-advice-text" class="text-sm text-gray-700"></div>
                </div>
            </div>

            <p class="text-gray-600 mt-8 mb-4">
                لنتائج الاستبيان وطباعتها اضغط على الأمر التالي في لوحة المفاتيح:
                <span class="font-bold text-gray-800">`ctrl+p`</span>
            </p>
            
            <p class="text-xs text-gray-500 mt-8">
                **ملاحظة:** هذا النموذج لا يقوم بتخزين أي بيانات شخصية أو بيانات الاستبيان.
            </p>
            
            <hr class="my-8">
            <div class="space-y-4 text-right">
                <h3 class="text-xl font-semibold text-gray-700 mb-4">💬 إرسال ملاحظات</h3>
                <form id="feedback-form" action="https://formspree.io/f/xjkeqqoe" method="POST">
                    <input type="hidden" name="subject" value="تغذية راجعة عن استبيان المعتقدات الشخصية">
                    <input type="text" name="name" placeholder="اسمك (اختياري)" class="form-input mb-4">
                    <input type="email" name="email" placeholder="بريدك الإلكتروني (اختياري)" class="form-input mb-4">
                    <textarea id="feedback-text" name="message" rows="4" class="form-input" placeholder="الرجاء كتابة ملاحظاتك أو الأخطاء التي واجهتها هنا..." required></textarea>
                    <button type="submit" class="w-full px-6 py-3 bg-orange-500 text-white font-bold rounded-lg hover:bg-orange-600 transition-colors mt-4">
                        إرسال
                    </button>
                </form>
                <!-- رسائل التأكيد -->
                <div id="feedback-success" class="hidden mt-4 p-4 bg-green-100 border border-green-400 text-green-700 rounded-lg">
                    ✅ تم إرسال ملاحظاتك بنجاح! شكراً لمساهمتك.
                </div>
                <div id="feedback-error" class="hidden mt-4 p-4 bg-red-100 border border-red-400 text-red-700 rounded-lg">
                    ❌ حدث خطأ غير متوقع. حاول مرة أخرى.
                </div>
            </div>
            <p class="text-sm text-gray-500 mt-8">© Aloshari</p>
        </div>

    </div>

    <script>
        const questions = [
            ".1 من غير المحتمل أن أتعرض لموقف أظهر فيه بمظهر دوني أو قاصر",
            "2 يجب أن أتجنب المواقف المؤذية بأي ثمن",
            "3 إذا تعامل الناس معي بشكل ودود ، فقد يعني هذا أنهم يحاولون استغلالي",
            "4. يجب أن أقاوم تسلط المسؤلين على أن أحظى برضاهم وقبولهم في نفس الوقت.",
            "5 لا يمكنني تحمل المشاعر المؤذية.",
            "6. لا يمكنني التسامح مع الأخطاء أو العيوب أو النقائص.",
            "7. الآخرون عادة ما يكثرون من مطالبهم أكثر من اللازم.",
            "8. يجب أن أكون مركز اهتمام الآخرين.",
            "9. إذا لم أعتمد على أنظمة محددة في حياتي، فسينهار كل شيء من حولى",
            "10. لا أحتمل ألا أحظى بالاحترام الذي أستحقه، أو لا أحصل على ما استحقه.",
            "11. من المهم بالنسبة لي أن أؤدي عملا متكاملا طوال الوقت",
            "12. إنني أستمتع القيام بالأمور بمفردي أكثر مما يشاركني إياها الآخرون.",
            "13. إن لم آخذ حذري، فسيحاول الآخرون استغلالي أو التلاعب بي.",
            "14. للآخرين دوافع خفية",
            "15. إن أسوأ ما يمكن أن يحدث لي هو أن أتعرض للهجر.",
            "16. يجب أن يدرك الآخرون مدى تميزي.",
            "17. سيحاول الآخرون الحط من قدري عن عمد.",
            "18. إنني في حاجة لمساعدة الآخرين في اتخاذ قراراتي أو املاء ما يجب أن أفعله علي.",
            "19. التفاصيل غاية في الأهمية بالنسبة لي",
            "20. إذا نظرت للآخرين على أنهم متسلطون أكثر من اللازم، يكون لدي الحق في تجاهل مطالبهم.",
            "21. الشخصيات المسؤلة تميل إلى التطفل ، وكثرة المطالب ، والتدخل والهيمنة.",
            "22. إن الوسيلة الوحيدة لكي أحصل على بغيتي هي ابهار أو تسلية الآخرين.",
            "23. يجب أن أقوم بكل ما يمكنني أن أنجو بعمله دون أن يترتب عليه تبعات وخيمة.",
            "24. إذا علم الآخرون أشياء عنى ، فسيستخدمونها ضدي.",
            "25. إن العلاقات فوضوية وتتعارض مع الحرية.",
            "26. لا يفهمني سوى الأشخاص الذين يتمتعون بنفس قدر ذكائي والمعيتي.",
            "27. إنني جدير بمعاملة وامتيازات خاصة طالما أنني رفيع المنزلة.",
            "28 من المهم بالنسبة لي أن أكون حرا ومستقلا عن الآخرين من حولي.",
            "29 من الأفضل أن أترك وحدي في الكثير من المواقف.",
            "30 من الضروري الالتزام بأرفع المعايير في كل الأوقات، وإلا انهار كل شيء.",
            "31 المشاعر البغيضة ستتفاقم وسيصعب السيطرة عليها",
            "32. إننا نعيش في غابة، البقاء فيها للقوي",
            "33. يجب أن أتجنب المواقف التي أجذب فيها انتباه الآخرين ، أو أتحرى أقل قدر ممكن من الظهور.",
            "34. إن لم أشرك الآخرين في أمري ، فلن أروق لهم.",
            "35. إذا أردت شيئا، يجب أن أبذل قصارى جهدي للحصول عليه.",
            "36 من الأفضل أن أكون وحيدا على أن أظل ( حبيس) علاقات مع الآخرين.",
            "37. إن لم أسل الآخرين أو أبهرهم، فأنا نكرة.",
            "38. سيقوم الآخرون بمهاجمتي إن لم أهاجمهم أنا أولا.",
            "39. إن أية أمارات للتوتر تشوب علاقة ما، هي مؤشر على أن العلاقة فسدت ولذا يجب على إنهاؤها",
            "40. إن يبلغ أدائي ذروته، فسأفشل.",
            "41. تحديد مواعيد انهاء، والإذعان للمطالب ، والامتثال كلها ضربات موجعة لكبريائي واكتفائي الذاتي.",
            "42. لقد تعرضت للظلم، ولذا فإنني مخول لأن أحصل على نصيبي العادل بغض النظر عن الوسيلة.",
            "43. إذا تقرب الآخرون، فسيكتشفون حقيقتي وينبذونني.",
            "44 إنني معوز وضعيف.",
            "45. عندما يتركوني وحيدا ، أجدني عاجزا",
            "46. يجب أن يلبي الآخرون حاجاتي",
            "47. إذا اتبعت القواعد بالشكل الذي يتوقعه الآخرون، فسيحد هذا من حريتي.",
            "48 سيستغلني الآخرون إذا ما سمحت لهم بهذا",
            "49. يجب أن أكون متحفزا طوال الوقت.",
            "50 خصوصيتي أهم بكثير لدي من القرب من الآخرين.",
            "51. القواعد اعتباطية وتقيدني.",
            "52. إنه لأمر شنيع أن يتجاهلني الآخرون.",
            "53. لا يهمني رأي الآخرين.",
            "54 لكي أحقق السعادة، أنا في حاجة لأن يوليني الآخرون اهتمامهم.",
            "55. إذا حرصت على تسلية الآخرين، فلن يلاحظوا نقاط ضعفي",
            "56. إنني في حاجة إلى شخص يلازمني لمساعدتي في القيام بكل ما أحتاج إليه أو في حالة حدوث مكروه.",
            "57. من الممكن أن يؤدي أي عيب أو نقص في الأداء إلى كارثة.",
            "58. طالما أنني أتمتع بالموهبة الشديدة، يجب أن يتجشم الآخرون عناء تعزيز مشواري المهني.",
            "59. إن لم أمارس ضغوطا على الآخرين، فسيمارسون هم ضغوطا علي.",
            "60. ليس من الضروري أن ألتزم بالقواعد التي تنطبق على الآخرين.",
            "61. إن القوة والمكر هما أفضل السبل لانجاز الأمور.",
            "62. يجب أن أحافظ على قنوات الاتصال بيني وبين نصيري أو مساعدي طوال الوقت",
            "63. إنني وحيد في الأساس، إلا إذا استطعت أن أقيم رابطا بيني وبين شخص قوي.",
            "64. لا يمكنني الثقة بالآخرين.",
            "65. لا أستطيع التكيف."
        ];

        const scoreKey = {
            "الاجتنابي": [1, 2, 5, 31, 33, 39, 43],
            "الاعتمادي": [15, 18, 44, 45, 56, 62, 63],
            "السلبي العدواني": [4, 7, 20, 21, 41, 47, 51],
            "الوسواس القهري": [6, 9, 11, 19, 30, 40, 57],
            "المضاد للمجتمع": [23, 32, 35, 38, 42, 59, 61],
            "النرجسي": [10, 16, 26, 27, 46, 58, 60],
            "المتصنع (الاستعراضي)": [8, 22, 34, 37, 52, 54, 55],
            "الفصامي": [12, 25, 28, 29, 36, 50, 53],
            "المصاب بجنون الاضطهاد": [3, 13, 14, 17, 24, 48, 49],
            "الحدودي": [31, 44, 45, 49, 56, 64, 65]
        };

        const patternsDescriptions = {
            "الاجتنابي": "إن الأشخاص الذين يندرجون تحت هذا النمط من أنماط الشخصية عادة ما يعانون من تدني احترامهم لذاتهم ويبدون حساسية شديدة تجاه النقد. ويمكنهم إقامة علاقات مع الآخرين بمجرد أن يثبت لهم أنهم قادرون على الثقة بهم.",
            "الاعتمادي": "إن الأشخاص الذين يندرجون تحت هذا النمط من أنماط الشخصية عادة ما يتشبثون بالعلاقات ويبذلون قصارى جهدهم للابقاء عليها. وينتابهم القلق من أن يتعرضو للهجر والوحدة. وعادة ما يشعرون بالعجز عن الحياة في حالة عدم وجود شخص ما بجوارهم، عادة ما يكون شخصا يعتقدون أنه سيتولاهم بالرعاية من الممكن لهؤلاء الأشخاص الذين يندرجون تحت هذه الشخصية أن يبدوا قدرا كبيرا من الاخلاص والولاء.",
            "السلبي العدواني": "هؤلاء الأشخاص لديهم مشاعر مختلطة حول الاستمرار في الأشياء . فقد يقولون أنهم سيفعلون شيئا ، ولكنهم في الغالب لا يستمرون فيه ويبدوا أن لديهم توجه عقلي غير مبال بالمواعيد النهائية والقواعد.",
            "الوسواس القهري": "إن هؤلاء الأشخاص مكرسون بشدة للعمل والإنتاجية . وعادة ما يضعون قوائم، ويحافظون على جدول أعمال مزدحم ، ويرصدون لأنفسهم وللآخرين معايير رفيعة. وعادة ما ينظر إليهم الآخرون على أنهم أشخاص أمناء ويعتمد عليهم، بيد أنهم أحيانا ما ينظر إليهم على أنهم مفرطون في تكريس أنفسهم للعمل . وبعض هؤلاء الأشخاص يدخرون الأشياء ظنا منهم أنها ستفيدهم بشكل أو بآخر في يوم من الأيام.",
            "المضاد للمجتمع": "هؤلاء الأشخاص يحبون الاثارة والمخاطرة. وعادة ما يعتقدون أن القواعد لا تنطبق عليهم ، ومن المحتمل أن يضربوا بهذه القواعد عرض الحائط لتحقيق بغيتهم. وهم يسعون لتمضية أوقات صاخبة، وأحيانا ما يكون من الممتع التواجد بصحبتهم. ويبدوا أنهم لا يولون بالا لحقوق وحاجات الآخرين.",
            "النرجسي": "هؤلاء الأشخاص يظنون أنهم أرفع مكانة من الآخرين، ويستحقون اهتماما خاصا وإعجابا من نوع فريد . وهم كثيرا ما يتعاملون بفظاظة مع الآخرين، وأحيانا لا يعون أنهم يسيؤن للآخرين . ولأنهم يتمتعون بقد كبير من الثقة بالنفس، قد تكون لديهم القدرة على تحقيق انجازات ، على الرغم من أن ثقتهم قد لا يكون لها أساس واقعي.",
            "المتصنع (الاستعراضي)": "إن هؤلاء الأشخاص يتمتعون بقدر كبير من الدراتيكية ويحاولون ابهار من حولهم برونقهم وشخصيتهم. وقد يبدون في منهى العاطفية، وهو ما يمكن أن يضيف إلى مدى إثارتهم للاهتمام . وهم يتمتعون بقدر كبير من الخيال والطاقة وعادة ما ينصب تركيزهم على مظهرهم.",
            "الفصامي": "هؤلاء الأشخاص يظنون أنهم أرفع مكانة من الآخرين، ويستحقون اهتماما خاصا وإعجابا من نوع فريد . وهم كثيرا ما يتعاملون بفظاظة مع الآخرين، وأحيانا لا يعون أنهم يسيؤن للآخرين . ولأنهم يتمتعون بقد كبير من الثقة بالنفس، قد تكون لديهم القدرة على تحقيق انجازات ، على الرغم من أن ثقتهم قد لا يكون لها أساس واقعي.",
            "المصاب بجنون الاضطهاد": "هؤلاء الأشخاص يحبون الاثارة والمخاطرة. وعادة ما يعتقدون أن القواعد لا تنطبق عليهم ، ومن المحتمل أن يضربوا بهذه القواعد عرض الحائط لتحقيق بغيتهم. وهم يسعون لتمضية أوقات صاخبة، وأحيانا ما يكون من الممتع التواجد بصحبتهم. ويبدوا أنهم لا يولون بالا لحقوق وحاجات الآخرين.",
            "الحدودي": "إن الأشخاص الذين يندرجون تحت هذا النمط من أنماط الشخصية عادة ما يعانون من تدني احترامهم لذاتهم ويبدون حساسية شديدة تجاه النقد. ويمكنهم إقامة علاقات مع الآخرين بمجرد أن يثبت لهم أنهم قادرون على الثقة بهم."
        };

        const questionsPerPage = 10;
        let currentPage = 0;
        const totalPages = Math.ceil(questions.length / questionsPerPage);
        const answers = {};
        let calculatedScores = {};

        const surveyContent = document.getElementById('survey-content');
        const prevBtn = document.getElementById('prev-btn');
        const nextBtn = document.getElementById('next-btn');
        const submitBtn = document.getElementById('submit-btn');
        const resultsPage = document.getElementById('results-page');
        const initialPage = document.getElementById('page-0');
        const feedbackForm = document.getElementById('feedback-form');
        const feedbackSuccess = document.getElementById('feedback-success');
        const feedbackError = document.getElementById('feedback-error');

        function generateSurveyPages() {
            surveyContent.innerHTML = '';
            for (let i = 0; i < totalPages; i++) {
                const page = document.createElement('div');
                page.className = 'page';
                page.id = `page-${i + 1}`;
                
                // Progress Bar
                const progressDiv = document.createElement('div');
                progressDiv.className = 'progress-bar-container mb-6';
                progressDiv.innerHTML = `
                    <div class="flex justify-between items-center mb-2 text-sm text-gray-600">
                        <span>التقدم: ${i + 1} من ${totalPages}</span>
                    </div>
                    <div class="progress-bar">
                        <div class="progress-fill" style="width: ${((i + 1) / totalPages) * 100}%;"></div>
                    </div>
                `;
                page.appendChild(progressDiv);

                const start = i * questionsPerPage;
                const end = Math.min(start + questionsPerPage, questions.length);

                for (let j = start; j < end; j++) {
                    const questionDiv = document.createElement('div');
                    questionDiv.className = 'mb-6 card p-4';
                    questionDiv.innerHTML = `
                        <p class="font-semibold text-gray-800 mb-4">${questions[j]}</p>
                        <div class="grid grid-cols-2 sm:grid-cols-5 gap-2 text-sm text-center">
                            <label class="col-span-1">
                                <input type="radio" name="q${j}" value="0" class="hidden" required>
                                <span class="radio-label">
                                    لا أؤمن أبداً
                                    <span class="block mt-1 text-gray-400 text-xs">(0)</span>
                                </span>
                            </label>
                            <label class="col-span-1">
                                <input type="radio" name="q${j}" value="1" class="hidden">
                                <span class="radio-label">
                                    أؤمن بقدر بسيط
                                    <span class="block mt-1 text-gray-400 text-xs">(1)</span>
                                </span>
                            </label>
                            <label class="col-span-1">
                                <input type="radio" name="q${j}" value="2" class="hidden">
                                <span class="radio-label">
                                    أؤمن إلى حد ما
                                    <span class="block mt-1 text-gray-400 text-xs">(2)</span>
                                </span>
                            </label>
                            <label class="col-span-1">
                                <input type="radio" name="q${j}" value="3" class="hidden">
                                <span class="radio-label">
                                    أؤمن كثيراً
                                    <span class="block mt-1 text-gray-400 text-xs">(3)</span>
                                </span>
                            </label>
                            <label class="col-span-1">
                                <input type="radio" name="q${j}" value="4" class="hidden">
                                <span class="radio-label">
                                    أؤمن تماماً
                                    <span class="block mt-1 text-gray-400 text-xs">(4)</span>
                                </span>
                            </label>
                        </div>
                    `;
                    page.appendChild(questionDiv);
                }

                surveyContent.appendChild(page);
            }
        }

        function showPage(pageNumber) {
            document.querySelectorAll('.page').forEach(page => page.classList.remove('active'));
            document.getElementById(`page-${pageNumber}`).classList.add('active');
            currentPage = pageNumber;
            updateButtons();
        }

        function updateButtons() {
            if (currentPage === 0) {
                prevBtn.style.display = 'none';
                nextBtn.style.display = 'none';
                submitBtn.style.display = 'none';
            } else {
                prevBtn.style.display = 'inline-block';
                if (currentPage === totalPages) {
                    nextBtn.style.display = 'none';
                    submitBtn.style.display = 'inline-block';
                } else {
                    nextBtn.style.display = 'inline-block';
                    submitBtn.style.display = 'none';
                }
            }
        }

        function saveAnswers() {
            const pageQuestions = document.querySelectorAll(`#page-${currentPage} input[type="radio"]:checked`);
            pageQuestions.forEach(input => {
                answers[parseInt(input.name.substring(1))] = parseInt(input.value);
            });
        }

        function isPageComplete() {
            const pageQuestions = document.querySelectorAll(`#page-${currentPage} input[type="radio"]`);
            const uniqueQuestions = new Set();
            pageQuestions.forEach(input => uniqueQuestions.add(input.name));
            let answeredQuestions = 0;
            uniqueQuestions.forEach(name => {
                if (document.querySelector(`input[name="${name}"]:checked`)) {
                    answeredQuestions++;
                }
            });
            return answeredQuestions === uniqueQuestions.size;
        }

        function nextPage() {
            if (currentPage === 0) {
                const nameInput = document.getElementById('name').value;
                const dateInput = document.getElementById('date').value;
                if (!nameInput || !dateInput) {
                    alert('الرجاء إدخال الاسم والتاريخ أولاً.');
                    return;
                }
                generateSurveyPages();
                initialPage.classList.remove('active');
                showPage(1);
            } else {
                if (!isPageComplete()) {
                    alert('الرجاء الإجابة على جميع الأسئلة في هذه الصفحة.');
                    return;
                }
                saveAnswers();
                if (currentPage < totalPages) {
                    showPage(currentPage + 1);
                }
            }
        }

        function prevPage() {
            if (currentPage > 1) {
                showPage(currentPage - 1);
            } else if (currentPage === 1) {
                showPage(0);
                initialPage.classList.add('active');
                prevBtn.style.display = 'none';
            }
        }

        function calculateScores() {
            const scores = {};
            for (const pattern in scoreKey) {
                scores[pattern] = 0;
                scoreKey[pattern].forEach(qNum => {
                    const qIndex = qNum - 1;
                    if (answers[qIndex] !== undefined) {
                        scores[pattern] += answers[qIndex];
                    }
                });
            }
            return scores;
        }

        function showResults() {
            if (!isPageComplete()) {
                alert('الرجاء الإجابة على جميع الأسئلة في هذه الصفحة.');
                return;
            }
            saveAnswers();

            calculatedScores = calculateScores();
            const resultsSummary = document.getElementById('results-summary');
            resultsSummary.innerHTML = `
                <div class="summary-box">
                    <div class="summary-item">
                        <span class="font-bold text-gray-800">الاسم:</span>
                        <span>${document.getElementById('name').value}</span>
                    </div>
                    <div class="summary-item">
                        <span class="font-bold text-gray-800">العمر:</span>
                        <span>${document.getElementById('age').value || 'لم يتم الإدخال'}</span>
                    </div>
                    <div class="summary-item">
                        <span class="font-bold text-gray-800">التاريخ:</span>
                        <span>${document.getElementById('date').value}</span>
                    </div>
                </div>
            `;
            
            const barChart = document.getElementById('bar-chart');
            barChart.innerHTML = '';
            const totalMaxScore = 7 * 4; // 7 questions per pattern * 4 max score
            for (const pattern in calculatedScores) {
                const score = calculatedScores[pattern];
                const percentage = (score / totalMaxScore) * 100;
                const barHtml = `
                    <div class="mb-4">
                        <div class="flex justify-between items-center mb-1">
                            <span class="font-medium text-gray-700">${pattern}</span>
                            <span class="text-sm font-semibold text-gray-600">${score}</span>
                        </div>
                        <div class="relative pt-1">
                            <div class="overflow-hidden h-6 text-xs flex rounded-full bg-gray-200">
                                <div class="w-full flex flex-col text-center justify-center text-white" style="width: ${percentage}%; background-color: #10B981;"></div>
                            </div>
                        </div>
                    </div>
                `;
                barChart.innerHTML += barHtml;
            }

            const patternsSummary = document.getElementById('patterns-summary');
            patternsSummary.innerHTML = '';
            for (const pattern in calculatedScores) {
                const description = patternsDescriptions[pattern];
                const summaryHtml = `
                    <div class="summary-box">
                        <h4 class="font-bold text-gray-800 mb-2">${pattern}</h4>
                        <p class="text-sm text-gray-600">${description}</p>
                    </div>
                `;
                patternsSummary.innerHTML += summaryHtml;
            }

            document.querySelectorAll('.page').forEach(page => page.classList.remove('active'));
            resultsPage.classList.add('active');
            document.getElementById('navigation-buttons').style.display = 'none';
        }

        // --- Gemini API Integration ---

        async function callGeminiAPI(prompt) {
            const apiKey = ""; 
            const apiUrl = "https://generativelanguage.googleapis.com/v1beta/models/gemini-2.5-flash-preview-05-20:generateContent?key=" + apiKey;

            let delay = 1000;
            for (let i = 0; i < 3; i++) {
                try {
                    const payload = {
                        contents: [{
                            parts: [{ text: prompt }]
                        }],
                        systemInstruction: {
                            parts: [{ text: "أنت خبير في علم النفس وتحليل الشخصية. قدم إجاباتك بطريقة احترافية ومباشرة." }]
                        }
                    };

                    const response = await fetch(apiUrl, {
                        method: "POST",
                        headers: { "Content-Type": "application/json" },
                        body: JSON.stringify(payload)
                    });

                    if (!response.ok) {
                        throw new Error(`API call failed with status: ${response.status}`);
                    }

                    const result = await response.json();
                    return result.candidates[0].content.parts[0].text;
                } catch (error) {
                    console.error("Attempt failed:", i, error);
                    if (i < 2) {
                        await new Promise(resolve => setTimeout(resolve, delay));
                        delay *= 2;
                    } else {
                        throw error;
                    }
                }
            }
        }

        async function generateGeminiSummary() {
            const container = document.getElementById('gemini-summary-container');
            const textElement = document.getElementById('gemini-summary-text');
            
            container.style.display = 'block';
            textElement.innerHTML = '... جاري إنشاء الملخص ✨';

            let prompt = "بناءً على نتائج استبيان المعتقدات الشخصية التالية، قدم ملخصًا وتحليلًا موجزًا للمستخدم، مع التركيز على أهم الأنماط التي ظهرت. النتائج هي: ";
            for (const pattern in calculatedScores) {
                prompt += `${pattern}: ${calculatedScores[pattern]} نقطة. `;
            }

            try {
                const summary = await callGeminiAPI(prompt);
                textElement.innerHTML = summary;
            } catch (error) {
                console.error("Failed to generate summary:", error);
                textElement.innerHTML = "حدث خطأ أثناء إنشاء الملخص. الرجاء المحاولة مرة أخرى.";
            }
        }

        async function generateBehavioralAdvice() {
            const container = document.getElementById('gemini-advice-container');
            const textElement = document.getElementById('gemini-advice-text');

            container.style.display = 'block';
            textElement.innerHTML = '... جاري إنشاء النصائح السلوكية ✨';

            let prompt = "بناءً على نتائج استبيان المعتقدات الشخصية التالية، قدم نصائح سلوكية عملية للمستخدم. ركز على أعلى درجات الأنماط وقدم توصيات محددة. النتائج هي: ";
            for (const pattern in calculatedScores) {
                prompt += `${pattern}: ${calculatedScores[pattern]} نقطة. `;
            }

            try {
                const advice = await callGeminiAPI(prompt);
                textElement.innerHTML = advice;
            } catch (error) {
                console.error("Failed to generate advice:", error);
                textElement.innerHTML = "حدث خطأ أثناء إنشاء النصائح. الرجاء المحاولة مرة أخرى.";
            }
        }

        // Handle feedback form submission with JavaScript
        feedbackForm.addEventListener('submit', async function(e) {
            e.preventDefault();
            
            const formData = new FormData(feedbackForm);
            const response = await fetch(feedbackForm.action, {
                method: feedbackForm.method,
                body: formData,
                headers: {
                    'Accept': 'application/json'
                }
            });

            if (response.ok) {
                feedbackSuccess.classList.remove('hidden');
                feedbackError.classList.add('hidden');
                feedbackForm.reset();
            } else {
                feedbackSuccess.classList.add('hidden');
                feedbackError.classList.remove('hidden');
            }
        });

    </script>
</body>
</html>
