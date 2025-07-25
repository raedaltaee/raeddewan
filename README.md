<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>نظام إدارة ومتابعة البريد الحكومي - ديوان الوقف الشيعي</title>

    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Cairo:wght@400;600;700&display=swap" rel="stylesheet">
    <style>
        body {
            font-family: 'Cairo', sans-serif;
            background-color: #f0f4f8;
        }
        .chart-container {
            position: relative;
            width: 100%;
            height: 300px;
        }
        .sidebar-icon {
            font-size: 1.5rem;
            line-height: 1;
        }
        .status-badge {
            padding: 0.25rem 0.75rem;
            border-radius: 9999px;
            font-size: 0.75rem;
            font-weight: 600;
        }
        .modal-overlay {
            position: fixed;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background-color: rgba(0, 0, 0, 0.5);
            display: flex;
            align-items: center;
            justify-content: center;
            z-index: 50;
        }
        .modal-content {
            background-color: white;
            padding: 2rem;
            border-radius: 0.75rem;
            width: 90%;
            max-width: 600px;
            max-height: 90vh;
            overflow-y: auto;
        }
    </style>
</head>
<body class="bg-slate-100">

    <div class="flex min-h-screen">
        <!-- Sidebar -->
        <aside class="w-64 bg-white text-gray-800 p-4 shadow-lg flex-shrink-0">
            <div class="text-center py-4 border-b">
                <h1 class="text-xl font-bold text-slate-800">ديوان الوقف الشيعي</h1>
                <p class="text-sm text-slate-500">نظام البريد الحكومي</p>
            </div>
            <nav class="mt-8">
                <ul>
                    <li><a href="#dashboard" class="nav-link flex items-center p-3 my-1 bg-sky-100 text-sky-700 rounded-lg font-bold"><span class="sidebar-icon me-3">📊</span> لوحة التحكم</a></li>
                    <li><a href="#mail" class="nav-link flex items-center p-3 my-1 hover:bg-sky-100 hover:text-sky-700 rounded-lg font-semibold"><span class="sidebar-icon me-3">✉️</span> إدارة البريد</a></li>
                    <li><a href="#hr" class="nav-link flex items-center p-3 my-1 hover:bg-sky-100 hover:text-sky-700 rounded-lg font-semibold"><span class="sidebar-icon me-3">👥</span> الموظفين</a></li>
                    <li><a href="#reports" class="nav-link flex items-center p-3 my-1 hover:bg-sky-100 hover:text-sky-700 rounded-lg font-semibold"><span class="sidebar-icon me-3">📈</span> التقارير</a></li>
                </ul>
            </nav>
            <div class="mt-auto text-center pt-8">
                 <p class="text-xs text-gray-500">تصميم وتطوير: ر. مهندسين</p>
                 <p class="text-sm font-semibold text-gray-700">رائد إبراهيم خليل</p>
            </div>
        </aside>

        <!-- Main Content -->
        <div class="flex-1 flex flex-col">
            <!-- Header -->
            <header class="bg-white shadow-sm p-4 flex justify-between items-center">
                <div id="header-title" class="text-xl font-bold text-slate-700">لوحة التحكم</div>
                <div class="flex items-center">
                    <span class="text-sm font-semibold">مرحباً, مدير النظام</span>
                    <button class="bg-red-500 text-white px-4 py-2 rounded-lg ms-4 hover:bg-red-600 transition-colors">خروج</button>
                </div>
            </header>

            <!-- Content Area -->
            <main class="flex-1 p-6 overflow-y-auto">
                <!-- Dashboard View -->
                <div id="dashboard-view" class="view">
                    <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-6 mb-6">
                        <div class="bg-white p-5 rounded-xl shadow text-center">
                            <h3 class="text-gray-500 font-semibold">إجمالي المعاملات</h3>
                            <p id="total-mail-kpi" class="text-4xl font-bold text-sky-600 mt-2">0</p>
                        </div>
                        <div class="bg-white p-5 rounded-xl shadow text-center">
                            <h3 class="text-gray-500 font-semibold">معاملات جديدة</h3>
                            <p id="new-mail-kpi" class="text-4xl font-bold text-amber-500 mt-2">0</p>
                        </div>
                        <div class="bg-white p-5 rounded-xl shadow text-center">
                            <h3 class="text-gray-500 font-semibold">قيد الإجراء</h3>
                            <p id="progress-mail-kpi" class="text-4xl font-bold text-indigo-500 mt-2">0</p>
                        </div>
                        <div class="bg-white p-5 rounded-xl shadow text-center">
                            <h3 class="text-gray-500 font-semibold">معاملات منجزة</h3>
                            <p id="completed-mail-kpi" class="text-4xl font-bold text-green-500 mt-2">0</p>
                        </div>
                    </div>
                    <div class="grid grid-cols-1 lg:grid-cols-5 gap-6">
                        <div class="bg-white p-6 rounded-xl shadow lg:col-span-3">
                            <h3 class="font-bold text-lg mb-4">حجم البريد الشهري (آخر 6 أشهر)</h3>
                            <div class="chart-container" style="height: 250px;"><canvas id="mailVolumeChart"></canvas></div>
                        </div>
                        <div class="bg-white p-6 rounded-xl shadow lg:col-span-2">
                            <h3 class="font-bold text-lg mb-4">حالة البريد</h3>
                            <div class="chart-container" style="height: 250px;"><canvas id="mailStatusChart"></canvas></div>
                        </div>
                    </div>
                </div>

                <!-- Mail View -->
                <div id="mail-view" class="view hidden">
                    <div class="bg-white p-6 rounded-xl shadow-md">
                        <div class="flex justify-between items-center mb-4 flex-wrap gap-4">
                            <h2 class="text-xl font-bold text-slate-700">سجل البريد الحكومي</h2>
                            <div class="flex items-center gap-2">
                                <input type="text" id="mailSearch" placeholder="ابحث في الموضوع أو الجهة..." class="border p-2 rounded-lg w-64">
                                <button id="add-mail-btn" class="bg-sky-600 text-white px-5 py-2 rounded-lg hover:bg-sky-700 font-bold">إضافة بريد جديد</button>
                            </div>
                        </div>
                        <div class="overflow-x-auto">
                            <table class="w-full text-right">
                                <thead class="border-b-2">
                                    <tr>
                                        <th class="p-3">رقم الكتاب</th>
                                        <th class="p-3">التاريخ</th>
                                        <th class="p-3">النوع</th>
                                        <th class="p-3">الجهة</th>
                                        <th class="p-3">الموضوع</th>
                                        <th class="p-3">الحالة</th>
                                        <th class="p-3">إجراءات</th>
                                    </tr>
                                </thead>
                                <tbody id="mailTableBody">
                                </tbody>
                            </table>
                        </div>
                    </div>
                </div>

                <!-- Mail Details View -->
                <div id="mail-details-view" class="view hidden">
                    <button class="back-to-mail mb-4 bg-gray-200 px-4 py-2 rounded-lg hover:bg-gray-300">&rarr; العودة إلى سجل البريد</button>
                    <div class="bg-white p-8 rounded-xl shadow-md" id="mail-details-content">
                    </div>
                </div>
                
                <!-- HR View -->
                <div id="hr-view" class="view hidden">
                     <div class="bg-white p-6 rounded-xl shadow-md">
                        <div class="flex justify-between items-center mb-4">
                            <h2 class="text-xl font-bold text-slate-700">قائمة الموظفين</h2>
                            <button id="add-employee-btn" class="bg-sky-600 text-white px-5 py-2 rounded-lg hover:bg-sky-700 font-bold">إضافة موظف</button>
                        </div>
                        <div class="overflow-x-auto">
                            <table class="w-full text-right">
                                <thead class="border-b-2">
                                    <tr>
                                        <th class="p-3">الاسم</th>
                                        <th class="p-3">المنصب</th>
                                        <th class="p-3">القسم</th>
                                        <th class="p-3">إجراءات</th>
                                    </tr>
                                </thead>
                                <tbody id="employeeTableBody">
                                </tbody>
                            </table>
                        </div>
                    </div>
                </div>

                <!-- Reports View -->
                <div id="reports-view" class="view hidden">
                    <h2 class="text-2xl font-bold text-slate-700 mb-2">التقارير والتحليلات</h2>
                    <p class="text-gray-600 mb-6">نظرة شاملة على أداء وكفاءة نظام البريد.</p>
                    
                    <div class="grid grid-cols-1 md:grid-cols-3 gap-6 mb-6">
                        <div class="bg-white p-5 rounded-xl shadow text-center">
                             <h3 class="text-gray-500 font-semibold">متوسط وقت الإنجاز</h3>
                             <p class="text-4xl font-bold text-cyan-600 mt-2">3.2 <span class="text-xl">أيام</span></p>
                        </div>
                         <div class="bg-white p-5 rounded-xl shadow">
                             <h3 class="text-gray-500 font-semibold text-center">توزيع البريد حسب الأولوية</h3>
                             <div class="chart-container" style="height: 120px"><canvas id="priority-report-chart"></canvas></div>
                        </div>
                        <div class="bg-white p-5 rounded-xl shadow">
                            <h3 class="text-gray-500 font-semibold text-center">توزيع البريد حسب النوع</h3>
                            <div class="chart-container" style="height: 120px"><canvas id="type-report-chart"></canvas></div>
                        </div>
                    </div>

                    <div class="bg-white p-6 rounded-xl shadow">
                        <h3 class="font-bold text-lg mb-4">تحليل أداء الموظفين (عدد المعاملات)</h3>
                        <div class="chart-container" style="height: 350px;"><canvas id="employee-performance-chart"></canvas></div>
                    </div>
                </div>
            </main>
        </div>
    </div>
    
    <!-- Mail Modal -->
    <div id="mail-modal" class="modal-overlay hidden">
        <div class="modal-content">
            <h2 id="mail-modal-title" class="text-2xl font-bold mb-6">إضافة بريد جديد</h2>
            <form id="mail-form">
                <input type="hidden" id="mail-id">
                <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                    <div>
                        <label for="mail-type" class="block font-semibold mb-1">نوع البريد</label>
                        <select id="mail-type" class="w-full border p-2 rounded-lg" required>
                            <option value="incoming">وارد</option>
                            <option value="outgoing">صادر</option>
                        </select>
                    </div>
                    <div>
                        <label for="mail-number" class="block font-semibold mb-1">رقم الكتاب</label>
                        <input type="text" id="mail-number" class="w-full border p-2 rounded-lg" required>
                    </div>
                    <div>
                        <label for="mail-date" class="block font-semibold mb-1">تاريخ الكتاب</label>
                        <input type="date" id="mail-date" class="w-full border p-2 rounded-lg" required>
                    </div>
                    <div>
                        <label for="mail-entity" class="block font-semibold mb-1">الجهة (الوارد منها / الصادر إليها)</label>
                        <input type="text" id="mail-entity" class="w-full border p-2 rounded-lg" required>
                    </div>
                    <div class="md:col-span-2">
                        <label for="mail-subject" class="block font-semibold mb-1">الموضوع</label>
                        <textarea id="mail-subject" rows="3" class="w-full border p-2 rounded-lg" required></textarea>
                    </div>
                    <div>
                        <label for="mail-priority" class="block font-semibold mb-1">الأولوية</label>
                        <select id="mail-priority" class="w-full border p-2 rounded-lg">
                            <option value="normal">عادي</option>
                            <option value="urgent">عاجل</option>
                        </select>
                    </div>
                     <div>
                        <label for="mail-assignee" class="block font-semibold mb-1">الموظف المسؤول</label>
                        <select id="mail-assignee" class="w-full border p-2 rounded-lg">
                        </select>
                    </div>
                     <div class="md:col-span-2">
                        <label for="mail-notes" class="block font-semibold mb-1">ملاحظات</label>
                        <textarea id="mail-notes" rows="2" class="w-full border p-2 rounded-lg"></textarea>
                    </div>
                </div>
                <div class="flex justify-end gap-4 mt-8">
                    <button type="button" class="modal-cancel-btn bg-gray-300 px-6 py-2 rounded-lg hover:bg-gray-400">إلغاء</button>
                    <button type="submit" class="bg-sky-600 text-white px-6 py-2 rounded-lg hover:bg-sky-700 font-bold">حفظ</button>
                </div>
            </form>
        </div>
    </div>

    <!-- Employee Modal -->
    <div id="employee-modal" class="modal-overlay hidden">
        <div class="modal-content">
            <h2 id="employee-modal-title" class="text-2xl font-bold mb-6">إضافة موظف جديد</h2>
            <form id="employee-form">
                <input type="hidden" id="employee-id">
                <div class="space-y-4">
                    <div>
                        <label for="employee-name" class="block font-semibold mb-1">اسم الموظف</label>
                        <input type="text" id="employee-name" class="w-full border p-2 rounded-lg" required>
                    </div>
                    <div>
                        <label for="employee-title" class="block font-semibold mb-1">المنصب</label>
                        <input type="text" id="employee-title" class="w-full border p-2 rounded-lg" required>
                    </div>
                    <div>
                        <label for="employee-department" class="block font-semibold mb-1">القسم</label>
                        <input type="text" id="employee-department" class="w-full border p-2 rounded-lg" required>
                    </div>
                </div>
                <div class="flex justify-end gap-4 mt-8">
                    <button type="button" class="modal-cancel-btn bg-gray-300 px-6 py-2 rounded-lg hover:bg-gray-400">إلغاء</button>
                    <button type="submit" class="bg-sky-600 text-white px-6 py-2 rounded-lg hover:bg-sky-700 font-bold">حفظ</button>
                </div>
            </form>
        </div>
    </div>
    
    <!-- Confirm Delete Modal -->
    <div id="confirm-modal" class="modal-overlay hidden">
        <div class="modal-content text-center max-w-sm">
            <h2 id="confirm-modal-title" class="text-xl font-bold mb-4">تأكيد الحذف</h2>
            <p id="confirm-modal-body" class="mb-6">هل أنت متأكد من أنك تريد حذف هذا العنصر؟ لا يمكن التراجع عن هذا الإجراء.</p>
            <div class="flex justify-center gap-4">
                <button id="confirm-cancel-btn" class="bg-gray-300 px-6 py-2 rounded-lg hover:bg-gray-400">إلغاء</button>
                <button id="confirm-delete-btn" class="bg-red-600 text-white px-6 py-2 rounded-lg hover:bg-red-700 font-bold">حذف</button>
            </div>
        </div>
    </div>


<script>
document.addEventListener('DOMContentLoaded', function () {
    // --- MOCK DATA ---
    let employeeData = [
        { id: 1, name: 'علي حسن', title: 'مهندس مدني', department: 'الهندسة'},
        { id: 2, name: 'فاطمة محمد', title: 'محاسب', department: 'المالية'},
        { id: 3, name: 'أحمد ياسين', title: 'باحث شرعي', department: 'الشؤون الشرعية'},
        { id: 4, name: 'زينب كريم', title: 'مسؤول إداري', department: 'الإدارة'}
    ];
    let mailData = [
        { id: 1, type: 'incoming', number: '120/أ', date: '2025-06-10', entity: 'محافظة بغداد', subject: 'بخصوص تخصيص قطعة أرض للوقف', status: 'completed', priority: 'urgent', assigneeId: 3, notes: 'تم الرد بتاريخ 2025-06-15', history: [{action: 'تم الإنشاء', by: 'مدير النظام', date: '2025-06-10'}, {action: 'تم الإنجاز والرد', by: 'أحمد ياسين', date: '2025-06-15'}] },
        { id: 2, type: 'outgoing', number: '345/ب', date: '2025-06-12', entity: 'وزارة المالية', subject: 'طلب بيان مالي للربع الثاني', status: 'in-progress', priority: 'normal', assigneeId: 2, notes: 'قيد المراجعة من القسم المالي', history: [{action: 'تم الإنشاء', by: 'مدير النظام', date: '2025-06-12'}] },
        { id: 3, type: 'incoming', number: '135/ج', date: '2025-06-15', entity: 'أمانة بغداد', subject: 'استفسار حول ترميم وقف تاريخي', status: 'in-progress', priority: 'normal', assigneeId: 1, notes: '', history: [{action: 'تم الإنشاء', by: 'مدير النظام', date: '2025-06-15'}] },
        { id: 4, type: 'incoming', number: '140/د', date: '2025-06-20', entity: 'مواطن', subject: 'شكوى بخصوص استغلال أرض وقف', status: 'new', priority: 'urgent', assigneeId: null, notes: 'تحتاج إلى تحقيق عاجل', history: [{action: 'تم الإنشاء', by: 'مدير النظام', date: '2025-06-20'}] },
        { id: 5, type: 'outgoing', number: '350/ب', date: '2025-06-21', entity: 'وزارة العدل', subject: 'متابعة دعوى قضائية', status: 'completed', priority: 'normal', assigneeId: 3, notes: 'تم إرسال المستندات المطلوبة.', history: [{action: 'تم الإنشاء', by: 'مدير النظام', date: '2025-06-21'}]}
    ];

    // --- DOM ELEMENTS ---
    const navLinks = document.querySelectorAll('.nav-link');
    const views = document.querySelectorAll('.view');
    const headerTitle = document.getElementById('header-title');

    // --- NAVIGATION ---
    function showView(viewId) {
        views.forEach(view => view.classList.add('hidden'));
        const activeView = document.getElementById(viewId + '-view');
        if (activeView) activeView.classList.remove('hidden');
        
        if (viewId === 'dashboard') updateDashboard();
        if (viewId === 'reports') updateReports();
    }

    navLinks.forEach(link => {
        link.addEventListener('click', function (e) {
            e.preventDefault();
            const viewId = this.getAttribute('href').substring(1);
            navLinks.forEach(l => l.classList.remove('bg-sky-100', 'text-sky-700', 'font-bold'));
            this.classList.add('bg-sky-100', 'text-sky-700', 'font-bold');
            headerTitle.textContent = this.textContent;
            showView(viewId);
        });
    });
    
    // --- STATUS HELPERS ---
    const getStatusInfo = (status) => {
        switch(status) {
            case 'new': return { text: 'جديد', color: 'bg-amber-100 text-amber-800' };
            case 'in-progress': return { text: 'قيد الإجراء', color: 'bg-indigo-100 text-indigo-800' };
            case 'completed': return { text: 'منجز', color: 'bg-green-100 text-green-800' };
            default: return { text: 'غير معروف', color: 'bg-gray-100 text-gray-800' };
        }
    };
    
    // --- CONFIRMATION MODAL ---
    const confirmModal = document.getElementById('confirm-modal');
    const confirmDeleteBtn = document.getElementById('confirm-delete-btn');
    const confirmCancelBtn = document.getElementById('confirm-cancel-btn');
    let deleteFunction = null;

    function showConfirmModal(onConfirm) {
        deleteFunction = onConfirm;
        confirmModal.classList.remove('hidden');
    }

    function hideConfirmModal() {
        confirmModal.classList.add('hidden');
        deleteFunction = null;
    }

    confirmDeleteBtn.addEventListener('click', () => {
        if (typeof deleteFunction === 'function') {
            deleteFunction();
        }
        hideConfirmModal();
    });
    confirmCancelBtn.addEventListener('click', hideConfirmModal);


    // --- MAIL MANAGEMENT ---
    const mailTableBody = document.getElementById('mailTableBody');
    const mailModal = document.getElementById('mail-modal');
    const mailForm = document.getElementById('mail-form');
    
    function populateMailTable(filter = '') {
        mailTableBody.innerHTML = '';
        const filteredData = mailData.filter(mail => mail.subject.includes(filter) || mail.entity.includes(filter));
        
        filteredData.forEach(mail => {
            const statusInfo = getStatusInfo(mail.status);
            const row = document.createElement('tr');
            row.className = 'border-b hover:bg-gray-50';
            row.innerHTML = `
                <td class="p-3 font-mono">${mail.number}</td>
                <td class="p-3">${mail.date}</td>
                <td class="p-3">${mail.type === 'incoming' ? 'وارد' : 'صادر'}</td>
                <td class="p-3">${mail.entity}</td>
                <td class="p-3">${mail.subject.substring(0, 40)}...</td>
                <td class="p-3"><span class="status-badge ${statusInfo.color}">${statusInfo.text}</span></td>
                <td class="p-3">
                    <button class="view-details-btn text-sky-600 hover:underline" data-id="${mail.id}">تفاصيل</button>
                    <button class="edit-mail-btn text-green-600 hover:underline ms-2" data-id="${mail.id}">تعديل</button>
                </td>
            `;
            mailTableBody.appendChild(row);
        });
    }
    
    function showMailDetails(id) {
        const mail = mailData.find(m => m.id === id);
        if (!mail) return;
        
        const contentDiv = document.getElementById('mail-details-content');
        const statusInfo = getStatusInfo(mail.status);
        const assignee = employeeData.find(e => e.id === mail.assigneeId);
        
        const historyHTML = mail.history.map(h => `<p class="text-sm text-gray-500">${h.date}: ${h.action} (بواسطة ${h.by})</p>`).join('');

        contentDiv.innerHTML = `
            <div class="border-b pb-4 mb-4">
                <div class="flex justify-between items-start">
                    <h2 class="text-3xl font-bold text-slate-800">${mail.subject}</h2>
                    <span class="status-badge ${statusInfo.color}">${statusInfo.text}</span>
                </div>
                <p class="text-md text-slate-600">من/إلى: ${mail.entity}</p>
                <p class="text-sm text-gray-500">رقم الكتاب: ${mail.number} | تاريخ: ${mail.date} | أولوية: ${mail.priority === 'urgent' ? 'عاجل' : 'عادي'}</p>
            </div>
            <div>
                <h3 class="font-bold text-xl mb-2 text-sky-700">الموظف المسؤول</h3>
                <p class="mb-4">${assignee ? assignee.name : 'غير معين'}</p>
                
                <h3 class="font-bold text-xl mb-2 text-sky-700">ملاحظات</h3>
                <p class="mb-4 bg-gray-50 p-3 rounded-lg">${mail.notes || 'لا توجد ملاحظات.'}</p>
                
                <h3 class="font-bold text-xl mb-2 text-sky-700">سجل الإجراءات</h3>
                <div class="space-y-1">${historyHTML}</div>
            </div>
        `;
        showView('mail-details');
    }
    
    function openMailModal(id = null) {
        mailForm.reset();
        const assigneeSelect = document.getElementById('mail-assignee');
        assigneeSelect.innerHTML = '<option value="">غير معين</option>';
        employeeData.forEach(emp => {
            const option = document.createElement('option');
            option.value = emp.id;
            option.textContent = emp.name;
            assigneeSelect.appendChild(option);
        });

        if (id) {
            document.getElementById('mail-modal-title').textContent = 'تعديل بيانات البريد';
            const mail = mailData.find(m => m.id === id);
            if (!mail) return;
            document.getElementById('mail-id').value = mail.id;
            document.getElementById('mail-type').value = mail.type;
            document.getElementById('mail-number').value = mail.number;
            document.getElementById('mail-date').value = mail.date;
            document.getElementById('mail-entity').value = mail.entity;
            document.getElementById('mail-subject').value = mail.subject;
            document.getElementById('mail-priority').value = mail.priority;
            document.getElementById('mail-assignee').value = mail.assigneeId || '';
            document.getElementById('mail-notes').value = mail.notes;
        } else {
            document.getElementById('mail-modal-title').textContent = 'إضافة بريد جديد';
            document.getElementById('mail-id').value = '';
            document.getElementById('mail-date').value = new Date().toISOString().split('T')[0];
        }
        mailModal.classList.remove('hidden');
    }
    
    mailForm.addEventListener('submit', (e) => {
        e.preventDefault();
        const id = Number(document.getElementById('mail-id').value);
        const mailItem = {
            type: document.getElementById('mail-type').value,
            number: document.getElementById('mail-number').value,
            date: document.getElementById('mail-date').value,
            entity: document.getElementById('mail-entity').value,
            subject: document.getElementById('mail-subject').value,
            priority: document.getElementById('mail-priority').value,
            assigneeId: Number(document.getElementById('mail-assignee').value) || null,
            notes: document.getElementById('mail-notes').value,
        };

        if (id) {
            const index = mailData.findIndex(m => m.id === id);
            if (index > -1) mailData[index] = { ...mailData[index], ...mailItem };
        } else {
            mailItem.id = Date.now();
            mailItem.status = 'new';
            mailItem.history = [{action: 'تم الإنشاء', by: 'مدير النظام', date: new Date().toISOString().split('T')[0]}];
            mailData.unshift(mailItem);
        }
        populateMailTable();
        updateDashboard();
        mailModal.classList.add('hidden');
    });

    document.getElementById('mailSearch').addEventListener('keyup', (e) => populateMailTable(e.target.value));
    document.getElementById('add-mail-btn').addEventListener('click', () => openMailModal());
    mailTableBody.addEventListener('click', (e) => {
        if (e.target.closest('.view-details-btn')) showMailDetails(Number(e.target.closest('.view-details-btn').dataset.id));
        if (e.target.closest('.edit-mail-btn')) openMailModal(Number(e.target.closest('.edit-mail-btn').dataset.id));
    });
    document.querySelector('.back-to-mail').addEventListener('click', () => showView('mail'));


    // --- HR MANAGEMENT ---
    const employeeTableBody = document.getElementById('employeeTableBody');
    const employeeModal = document.getElementById('employee-modal');
    const employeeForm = document.getElementById('employee-form');
    
    function populateEmployeeTable() {
        employeeTableBody.innerHTML = '';
        employeeData.forEach(emp => {
            const row = document.createElement('tr');
            row.className = 'border-b hover:bg-gray-50';
            row.innerHTML = `
                <td class="p-3">${emp.name}</td>
                <td class="p-3">${emp.title}</td>
                <td class="p-3">${emp.department}</td>
                <td class="p-3">
                    <button class="edit-employee-btn text-green-600 hover:underline" data-id="${emp.id}">تعديل</button>
                    <button class="delete-employee-btn text-red-600 hover:underline ms-2" data-id="${emp.id}">حذف</button>
                </td>
            `;
            employeeTableBody.appendChild(row);
        });
    }

    function openEmployeeModal(id = null) {
        employeeForm.reset();
        if (id) {
            document.getElementById('employee-modal-title').textContent = 'تعديل بيانات الموظف';
            const emp = employeeData.find(e => e.id === id);
            if (!emp) return;
            document.getElementById('employee-id').value = emp.id;
            document.getElementById('employee-name').value = emp.name;
            document.getElementById('employee-title').value = emp.title;
            document.getElementById('employee-department').value = emp.department;
        } else {
            document.getElementById('employee-modal-title').textContent = 'إضافة موظف جديد';
            document.getElementById('employee-id').value = '';
        }
        employeeModal.classList.remove('hidden');
    }

    function deleteEmployee(id) {
        showConfirmModal(() => {
            employeeData = employeeData.filter(emp => emp.id !== id);
            populateEmployeeTable();
        });
    }

    employeeForm.addEventListener('submit', (e) => {
        e.preventDefault();
        const id = Number(document.getElementById('employee-id').value);
        const empItem = {
            name: document.getElementById('employee-name').value,
            title: document.getElementById('employee-title').value,
            department: document.getElementById('employee-department').value,
        };

        if (id) {
            const index = employeeData.findIndex(e => e.id === id);
            if (index > -1) employeeData[index] = { ...employeeData[index], ...empItem };
        } else {
            empItem.id = Date.now();
            employeeData.push(empItem);
        }
        populateEmployeeTable();
        employeeModal.classList.add('hidden');
    });

    document.getElementById('add-employee-btn').addEventListener('click', () => openEmployeeModal());
    employeeTableBody.addEventListener('click', (e) => {
        if (e.target.closest('.edit-employee-btn')) openEmployeeModal(Number(e.target.closest('.edit-employee-btn').dataset.id));
        if (e.target.closest('.delete-employee-btn')) deleteEmployee(Number(e.target.closest('.delete-employee-btn').dataset.id));
    });


    // --- UNIVERSAL MODAL CANCEL ---
    document.querySelectorAll('.modal-cancel-btn').forEach(btn => {
        btn.addEventListener('click', () => {
            btn.closest('.modal-overlay').classList.add('hidden');
        });
    });

    // --- CHART INSTANCES ---
    let mailStatusChartInstance, mailVolumeChartInstance, priorityReportChartInstance, typeReportChartInstance, employeePerformanceChartInstance;

    // --- DASHBOARD ---
    function updateDashboard() {
        const total = mailData.length;
        const newCount = mailData.filter(m => m.status === 'new').length;
        const progressCount = mailData.filter(m => m.status === 'in-progress').length;
        const completedCount = mailData.filter(m => m.status === 'completed').length;
        
        document.getElementById('total-mail-kpi').textContent = total;
        document.getElementById('new-mail-kpi').textContent = newCount;
        document.getElementById('progress-mail-kpi').textContent = progressCount;
        document.getElementById('completed-mail-kpi').textContent = completedCount;

        if (mailStatusChartInstance) mailStatusChartInstance.destroy();
        mailStatusChartInstance = new Chart(document.getElementById('mailStatusChart').getContext('2d'), {
            type: 'doughnut',
            data: {
                labels: ['جديد', 'قيد الإجراء', 'منجز'],
                datasets: [{ data: [newCount, progressCount, completedCount], backgroundColor: ['#f59e0b', '#4f46e5', '#22c55e'] }]
            },
            options: { responsive: true, maintainAspectRatio: false, plugins: { legend: { position: 'right' } } }
        });

        if (mailVolumeChartInstance) mailVolumeChartInstance.destroy();
        mailVolumeChartInstance = new Chart(document.getElementById('mailVolumeChart').getContext('2d'), {
            type: 'line',
            data: {
                labels: ['يناير', 'فبراير', 'مارس', 'أبريل', 'مايو', 'يونيو'],
                datasets: [{ label: 'عدد المعاملات', data: [12, 19, 15, 25, 22, 30], backgroundColor: '#0ea5e955', borderColor: '#0ea5e9', tension: 0.3, fill: true }]
            },
            options: { responsive: true, maintainAspectRatio: false }
        });
    }

    // --- REPORTS ---
    function updateReports() {
        // Priority Chart
        const urgentCount = mailData.filter(m => m.priority === 'urgent').length;
        const normalCount = mailData.filter(m => m.priority === 'normal').length;
        if (priorityReportChartInstance) priorityReportChartInstance.destroy();
        priorityReportChartInstance = new Chart(document.getElementById('priority-report-chart').getContext('2d'), {
            type: 'pie',
            data: {
                labels: ['عاجل', 'عادي'],
                datasets: [{ data: [urgentCount, normalCount], backgroundColor: ['#ef4444', '#3b82f6'] }]
            },
            options: { responsive: true, maintainAspectRatio: false, plugins: { legend: { display: false } } }
        });
        
        // Type Chart
        const incomingCount = mailData.filter(m => m.type === 'incoming').length;
        const outgoingCount = mailData.filter(m => m.type === 'outgoing').length;
        if (typeReportChartInstance) typeReportChartInstance.destroy();
        typeReportChartInstance = new Chart(document.getElementById('type-report-chart').getContext('2d'), {
            type: 'pie',
            data: {
                labels: ['وارد', 'صادر'],
                datasets: [{ data: [incomingCount, outgoingCount], backgroundColor: ['#8b5cf6', '#14b8a6'] }]
            },
            options: { responsive: true, maintainAspectRatio: false, plugins: { legend: { display: false } } }
        });

        // Employee Performance Chart
        const employeeTasks = {};
        employeeData.forEach(emp => {
            employeeTasks[emp.id] = { name: emp.name, completed: 0, inProgress: 0, new: 0 };
        });
        mailData.forEach(mail => {
            if (mail.assigneeId && employeeTasks[mail.assigneeId]) {
                if (mail.status === 'completed') employeeTasks[mail.assigneeId].completed++;
                else if (mail.status === 'in-progress') employeeTasks[mail.assigneeId].inProgress++;
                else if (mail.status === 'new') employeeTasks[mail.assigneeId].new++;
            }
        });

        const labels = Object.values(employeeTasks).map(e => e.name);
        const completedData = Object.values(employeeTasks).map(e => e.completed);
        const inProgressData = Object.values(employeeTasks).map(e => e.inProgress);
        const newData = Object.values(employeeTasks).map(e => e.new);
        
        if (employeePerformanceChartInstance) employeePerformanceChartInstance.destroy();
        employeePerformanceChartInstance = new Chart(document.getElementById('employee-performance-chart').getContext('2d'), {
            type: 'bar',
            data: {
                labels: labels,
                datasets: [
                    { label: 'منجز', data: completedData, backgroundColor: '#22c55e' },
                    { label: 'قيد الإجراء', data: inProgressData, backgroundColor: '#4f46e5' },
                    { label: 'جديد', data: newData, backgroundColor: '#f59e0b' }
                ]
            },
            options: { responsive: true, maintainAspectRatio: false, scales: { x: { stacked: true }, y: { stacked: true } } }
        });
    }


    // --- INITIAL LOAD ---
    populateMailTable();
    populateEmployeeTable();
    updateDashboard();
    showView('dashboard');
});
</script>

</body>
</html>
# raeddewan
