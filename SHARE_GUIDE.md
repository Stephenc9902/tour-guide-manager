<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>导览器管理系统</title>
    <!-- Tailwind CSS v3 -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Font Awesome -->
    <link href="https://cdn.jsdelivr.net/npm/font-awesome@4.7.0/css/font-awesome.min.css" rel="stylesheet">
    <!-- Chart.js -->
    <script src="https://cdn.jsdelivr.net/npm/chart.js@4.4.8/dist/chart.umd.min.js"></script>
    <!-- 统一的 Tailwind 配置 -->
    <script>
        tailwind.config = {
            theme: {
                extend: {
                    colors: {
                        primary: '#1e40af',
                        secondary: '#3b82f6',
                        available: '#10b981',
                        borrowed: '#f59e0b',
                        error: '#ef4444',
                        dark: '#1e293b',
                        light: '#f8fafc'
                    },
                    fontFamily: {
                        sans: ['Inter', 'system-ui', 'sans-serif'],
                    }
                }
            }
        }
    </script>
    <style>
        .nav-item {
            color: rgba(255, 255, 255, 0.8);
            text-decoration: none;
        }
        .nav-item:hover {
            background-color: rgba(255, 255, 255, 0.1);
            color: white;
        }
        .nav-item.active {
            background-color: rgba(255, 255, 255, 0.2);
            color: white;
            font-weight: 600;
        }
        .nav-item.active i {
            color: #34d399;
        }
    </style>
</head>
<body class="bg-gray-100 font-sans">
    <div class="flex h-screen overflow-hidden">
        <!-- 侧边导航栏 -->
        <aside class="w-64 bg-primary text-white hidden md:block flex flex-col">
            <div class="p-6 border-b border-blue-700">
                <h1 class="text-xl font-bold">导览器管理系统</h1>
                <p class="text-sm text-gray-300 mt-1">高效管理您的导览设备</p>
            </div>
            <nav class="mt-4 px-2 space-y-1 flex-1">
                <a href="#dashboard" class="nav-item active flex items-center px-4 py-3 rounded-lg transition-all duration-200" data-page="dashboard">
                    <i class="fa fa-dashboard w-6 text-center mr-3"></i>
                    <span class="font-medium">仪表盘</span>
                </a>
                <a href="#devices" class="nav-item flex items-center px-4 py-3 rounded-lg transition-all duration-200" data-page="devices">
                    <i class="fa fa-headphones w-6 text-center mr-3"></i>
                    <span class="font-medium">设备管理</span>
                </a>
                <a href="#borrow" class="nav-item flex items-center px-4 py-3 rounded-lg transition-all duration-200" data-page="borrow">
                    <i class="fa fa-exchange w-6 text-center mr-3"></i>
                    <span class="font-medium">借用归还</span>
                </a>
                <a href="#reports" class="nav-item flex items-center px-4 py-3 rounded-lg transition-all duration-200" data-page="reports">
                    <i class="fa fa-bar-chart w-6 text-center mr-3"></i>
                    <span class="font-medium">统计报表</span>
                </a>
                <a href="#guides" class="nav-item flex items-center px-4 py-3 rounded-lg transition-all duration-200" data-page="guides">
                    <i class="fa fa-users w-6 text-center mr-3"></i>
                    <span class="font-medium">导游管理</span>
                </a>
            </nav>
            <div class="p-4 border-t border-blue-700">
                <div class="text-sm text-gray-300">
                    <div class="flex items-center mb-1">
                        <i class="fa fa-info-circle w-5 text-center mr-2"></i>
                        <span>设备总数: <span class="font-semibold text-white" id="sidebar-total-devices">15</span></span>
                    </div>
                    <div class="flex items-center">
                        <i class="fa fa-check-circle w-5 text-center mr-2"></i>
                        <span>可用设备: <span class="font-semibold text-green-300" id="sidebar-available-devices">10</span></span>
                    </div>
                </div>
            </div>
        </aside>

        <!-- 移动端导航菜单按钮 -->
        <div class="md:hidden fixed bottom-4 right-4 z-50">
            <button id="mobile-menu-btn" class="bg-primary text-white w-12 h-12 rounded-full shadow-lg flex items-center justify-center">
                <i class="fa fa-bars"></i>
            </button>
        </div>

        <!-- 移动端导航菜单 -->
        <div id="mobile-menu" class="fixed inset-0 bg-primary z-40 transform translate-x-full transition-transform duration-300 md:hidden">
            <div class="p-6 flex justify-between items-center">
                <h1 class="text-2xl font-bold text-white">导览器管理系统</h1>
                <button id="close-mobile-menu" class="text-white text-2xl">
                    <i class="fa fa-times"></i>
                </button>
            </div>
            <nav class="mt-6 px-4 space-y-2">
                <a href="#dashboard" class="nav-item active text-white" data-page="dashboard">
                    <i class="fa fa-dashboard w-5 text-center mr-3"></i>
                    <span>仪表盘</span>
                </a>
                <a href="#devices" class="nav-item text-white" data-page="devices">
                    <i class="fa fa-headphones w-5 text-center mr-3"></i>
                    <span>设备管理</span>
                </a>
                <a href="#borrow" class="nav-item text-white" data-page="borrow">
                    <i class="fa fa-exchange w-5 text-center mr-3"></i>
                    <span>借用归还</span>
                </a>
                <a href="#reports" class="nav-item text-white" data-page="reports">
                    <i class="fa fa-bar-chart w-5 text-center mr-3"></i>
                    <span>统计报表</span>
                </a>
                <a href="#guides" class="nav-item text-white" data-page="guides">
                    <i class="fa fa-users w-5 text-center mr-3"></i>
                    <span>导游管理</span>
                </a>
            </nav>
        </div>

        <!-- 主内容区 -->
        <main class="flex-1 overflow-y-auto bg-gray-100">
            <!-- 顶部导航栏 -->
            <header class="bg-white shadow-sm">
                <div class="px-6 py-4 flex justify-between items-center">
                    <div>
                        <h2 id="page-title" class="text-2xl font-bold text-gray-900">仪表盘</h2>
                        <p id="page-subtitle" class="text-sm text-gray-500">欢迎回来，今天是 <span id="current-date"></span></p>
                    </div>
                </div>
            </header>

            <!-- 页面内容 -->
            <div class="p-6">
                <!-- 仪表盘页面 -->
                <div id="dashboard-page" class="page active">
                    <!-- 统计卡片 -->
                    <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-6 mb-8">
                        <div class="bg-white rounded-xl shadow p-6 cursor-pointer hover:shadow-lg transition-all duration-300 hover:-translate-y-1" onclick="showStatDetails('total')">
                            <div class="flex justify-between items-start">
                                <div>
                                    <p class="text-sm text-gray-500">总设备数</p>
                                    <p class="text-3xl font-bold" id="total-devices">15</p>
                                </div>
                                <div class="w-10 h-10 rounded-full bg-primary/10 flex items-center justify-center text-primary">
                                    <i class="fa fa-headphones"></i>
                                </div>
                            </div>
                            <div class="mt-2 text-xs text-gray-400 flex items-center">
                                <i class="fa fa-info-circle mr-1"></i>
                                <span>点击查看详情</span>
                            </div>
                        </div>
                        <div class="bg-white rounded-xl shadow p-6 cursor-pointer hover:shadow-lg transition-all duration-300 hover:-translate-y-1" onclick="showStatDetails('available')">
                            <div class="flex justify-between items-start">
                                <div>
                                    <p class="text-sm text-gray-500">可用设备</p>
                                    <p class="text-3xl font-bold text-available" id="available-devices">10</p>
                                </div>
                                <div class="w-10 h-10 rounded-full bg-available/10 flex items-center justify-center text-available">
                                    <i class="fa fa-check-circle"></i>
                                </div>
                            </div>
                            <div class="mt-2 text-xs text-gray-400 flex items-center">
                                <i class="fa fa-info-circle mr-1"></i>
                                <span>点击查看详情</span>
                            </div>
                        </div>
                        <div class="bg-white rounded-xl shadow p-6 cursor-pointer hover:shadow-lg transition-all duration-300 hover:-translate-y-1" onclick="showStatDetails('borrowed')">
                            <div class="flex justify-between items-start">
                                <div>
                                    <p class="text-sm text-gray-500">已借出</p>
                                    <p class="text-3xl font-bold text-borrowed" id="borrowed-devices">4</p>
                                </div>
                                <div class="w-10 h-10 rounded-full bg-borrowed/10 flex items-center justify-center text-borrowed">
                                    <i class="fa fa-exchange"></i>
                                </div>
                            </div>
                            <div class="mt-2 text-xs text-gray-400 flex items-center">
                                <i class="fa fa-info-circle mr-1"></i>
                                <span>点击查看详情</span>
                            </div>
                        </div>
                        <div class="bg-white rounded-xl shadow p-6 cursor-pointer hover:shadow-lg transition-all duration-300 hover:-translate-y-1" onclick="showStatDetails('error')">
                            <div class="flex justify-between items-start">
                                <div>
                                    <p class="text-sm text-gray-500">异常设备</p>
                                    <p class="text-3xl font-bold text-error" id="error-devices">1</p>
                                </div>
                                <div class="w-10 h-10 rounded-full bg-error/10 flex items-center justify-center text-error">
                                    <i class="fa fa-exclamation-triangle"></i>
                                </div>
                            </div>
                            <div class="mt-2 text-xs text-gray-400 flex items-center">
                                <i class="fa fa-info-circle mr-1"></i>
                                <span>点击查看详情</span>
                            </div>
                        </div>
                    </div>

                    <!-- 设备状态卡片 -->
                    <div class="bg-white rounded-xl shadow p-6 mb-8">
                        <div class="flex justify-between items-center mb-6">
                            <h3 class="text-lg font-bold text-gray-900">设备状态</h3>
                            <div class="flex gap-2">
                                <button id="dashboard-filter-btn" class="px-4 py-2 border border-gray-300 rounded-lg text-sm flex items-center gap-2 hover:bg-gray-50">
                                    <i class="fa fa-filter"></i>
                                    <span>筛选</span>
                                </button>
                                <button id="dashboard-refresh-btn" class="px-4 py-2 bg-primary text-white rounded-lg text-sm flex items-center gap-2 hover:bg-primary/90">
                                    <i class="fa fa-refresh"></i>
                                    <span>刷新</span>
                                </button>
                            </div>
                        </div>
                        <div class="grid grid-cols-2 sm:grid-cols-3 md:grid-cols-4 lg:grid-cols-5 gap-4" id="device-cards-container">
                            <!-- 设备卡片将通过 JavaScript 动态生成 -->
                        </div>
                    </div>

                    <!-- 最近活动 -->
                    <div class="bg-white rounded-xl shadow p-6">
                        <h3 class="text-lg font-bold text-gray-900 mb-6">最近活动</h3>
                        <div class="space-y-4" id="recent-activities">
                            <!-- 最近活动将通过 JavaScript 动态生成 -->
                        </div>
                    </div>
                </div>

                <!-- 设备管理页面 -->
                <div id="devices-page" class="page hidden">
                    <div class="bg-white rounded-xl shadow p-6 mb-8">
                        <div class="flex justify-between items-center mb-6">
                            <h3 class="text-lg font-bold text-gray-900">设备管理</h3>
                            <div class="flex gap-2">
                                <div class="relative">
                                    <input type="text" id="device-search" placeholder="搜索设备..." class="pl-10 pr-4 py-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-primary focus:border-transparent">
                                    <i class="fa fa-search absolute left-3 top-3 text-gray-400"></i>
                                </div>
                                <button id="device-filter-btn" class="px-4 py-2 border border-gray-300 rounded-lg text-sm flex items-center gap-2 hover:bg-gray-50">
                                    <i class="fa fa-filter"></i>
                                    <span>筛选</span>
                                </button>
                                <button id="add-device-btn" class="px-4 py-2 bg-primary text-white rounded-lg text-sm flex items-center gap-2 hover:bg-primary/90">
                                    <i class="fa fa-plus"></i>
                                    <span>添加</span>
                                </button>
                            </div>
                        </div>
                        <div class="overflow-x-auto">
                            <table class="min-w-full divide-y divide-gray-200">
                                <thead class="bg-gray-50">
                                    <tr>
                                        <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">设备编号</th>
                                        <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">状态</th>
                                        <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">位置</th>
                                        <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">使用人</th>
                                        <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">借出时间</th>
                                        <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">操作</th>
                                    </tr>
                                </thead>
                                <tbody class="bg-white divide-y divide-gray-200" id="devices-table-body">
                                    <!-- 设备列表将通过 JavaScript 动态生成 -->
                                </tbody>
                            </table>
                        </div>
                    </div>
                </div>

                <!-- 借用归还页面 -->
                <div id="borrow-page" class="page hidden">
                    <div class="grid grid-cols-1 lg:grid-cols-2 gap-6">
                        <!-- 借用设备 -->
                        <div class="bg-white rounded-xl shadow p-6">
                            <h3 class="text-lg font-bold text-gray-900 mb-6">借用设备</h3>
                            <form id="borrow-form">
                                <div class="mb-4">
                                    <label for="guide-select" class="block text-sm font-medium text-gray-700 mb-1">选择导游</label>
                                    <select id="guide-select" class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-primary focus:border-transparent" required>
                                        <option value="">请选择导游</option>
                                    </select>
                                </div>
                                <div class="mb-4">
                                    <label for="device-select" class="block text-sm font-medium text-gray-700 mb-1">选择设备</label>
                                    <select id="device-select" class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-primary focus:border-transparent" required>
                                        <option value="">请选择设备</option>
                                    </select>
                                </div>
                                <div class="mb-4">
                                    <label for="borrow-notes" class="block text-sm font-medium text-gray-700 mb-1">备注</label>
                                    <textarea id="borrow-notes" class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-primary focus:border-transparent" rows="3" placeholder="请输入备注信息（可选）"></textarea>
                                </div>
                                <div class="flex justify-end">
                                    <button type="submit" class="px-4 py-2 bg-primary text-white rounded-lg flex items-center gap-2 hover:bg-primary/90">
                                        <i class="fa fa-check"></i>
                                        <span>确认借用</span>
                                    </button>
                                </div>
                            </form>
                        </div>

                        <!-- 归还设备 -->
                        <div class="bg-white rounded-xl shadow p-6">
                            <h3 class="text-lg font-bold text-gray-900 mb-6">归还设备</h3>
                            <form id="return-form">
                                <div class="mb-4">
                                    <label for="return-device-select" class="block text-sm font-medium text-gray-700 mb-1">选择设备</label>
                                    <select id="return-device-select" class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-primary focus:border-transparent" required>
                                        <option value="">请选择设备</option>
                                    </select>
                                </div>
                                <div class="mb-4">
                                    <label for="return-location" class="block text-sm font-medium text-gray-700 mb-1">归还位置</label>
                                    <input type="text" id="return-location" class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-primary focus:border-transparent" value="办公室" required>
                                </div>
                                <div class="mb-4">
                                    <label for="return-notes" class="block text-sm font-medium text-gray-700 mb-1">备注</label>
                                    <textarea id="return-notes" class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-primary focus:border-transparent" rows="3" placeholder="请输入备注信息（可选）"></textarea>
                                </div>
                                <div class="flex justify-end">
                                    <button type="submit" class="px-4 py-2 bg-available text-white rounded-lg flex items-center gap-2 hover:bg-available/90">
                                        <i class="fa fa-check"></i>
                                        <span>确认归还</span>
                                    </button>
                                </div>
                            </form>
                        </div>
                    </div>
                </div>

                <!-- 统计报表页面 -->
                <div id="reports-page" class="page hidden">
                    <div class="bg-white rounded-xl shadow p-6 mb-8">
                        <h3 class="text-lg font-bold text-gray-900 mb-6">设备使用统计</h3>
                        <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
                            <div class="h-64">
                                <canvas id="device-status-chart"></canvas>
                            </div>
                            <div class="h-64">
                                <canvas id="guide-usage-chart"></canvas>
                            </div>
                        </div>
                    </div>
                </div>

                <!-- 导游管理页面 -->
                <div id="guides-page" class="page hidden">
                    <div class="bg-white rounded-xl shadow p-6 mb-8">
                        <div class="flex justify-between items-center mb-6">
                            <h3 class="text-lg font-bold text-gray-900">导游管理</h3>
                            <div class="flex gap-2">
                                <button id="add-guide-btn" class="px-4 py-2 bg-primary text-white rounded-lg text-sm flex items-center gap-2 hover:bg-primary/90">
                                    <i class="fa fa-plus"></i>
                                    <span>添加导游</span>
                                </button>
                            </div>
                        </div>
                        <div class="overflow-x-auto">
                            <table class="min-w-full divide-y divide-gray-200">
                                <thead class="bg-gray-50">
                                    <tr>
                                        <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">ID</th>
                                        <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">姓名</th>
                                        <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">联系电话</th>
                                        <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">状态</th>
                                        <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">操作</th>
                                    </tr>
                                </thead>
                                <tbody class="bg-white divide-y divide-gray-200" id="guides-table-body">
                                    <!-- 导游列表将通过 JavaScript 动态生成 -->
                                </tbody>
                            </table>
                        </div>
                    </div>
                </div>
            </div>
        </main>
    </div>

    <!-- 添加导游模态框 -->
    <div id="add-guide-modal" class="fixed inset-0 bg-black bg-opacity-50 z-50 flex items-center justify-center hidden">
        <div class="bg-white rounded-xl shadow-lg w-full max-w-md p-6">
            <div class="flex justify-between items-center mb-6">
                <h3 class="text-lg font-bold text-gray-900">添加导游</h3>
                <button id="close-guide-modal" class="text-gray-500 hover:text-gray-700">
                    <i class="fa fa-times"></i>
                </button>
            </div>
            <form id="guide-form">
                <div class="mb-4">
                    <label for="guide-name" class="block text-sm font-medium text-gray-700 mb-1">姓名</label>
                    <input type="text" id="guide-name" class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-primary focus:border-transparent" required>
                </div>
                <div class="mb-4">
                    <label for="guide-phone" class="block text-sm font-medium text-gray-700 mb-1">联系电话</label>
                    <input type="tel" id="guide-phone" class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-primary focus:border-transparent">
                </div>
                <div class="flex justify-end mt-6">
                    <button type="button" id="cancel-guide-btn" class="px-4 py-2 border border-gray-300 text-gray-700 rounded-lg mr-2 hover:bg-gray-50">取消</button>
                    <button type="submit" class="px-4 py-2 bg-primary text-white rounded-lg hover:bg-primary/90">保存</button>
                </div>
            </form>
        </div>
    </div>

    <!-- 添加设备模态框 -->
    <div id="add-device-modal" class="fixed inset-0 bg-black bg-opacity-50 z-50 flex items-center justify-center hidden">
        <div class="bg-white rounded-xl shadow-lg w-full max-w-md p-6">
            <div class="flex justify-between items-center mb-6">
                <h3 class="text-lg font-bold text-gray-900">添加设备</h3>
                <button id="close-add-device-modal" class="text-gray-500 hover:text-gray-700">
                    <i class="fa fa-times"></i>
                </button>
            </div>
            <form id="add-device-form">
                <div class="mb-4">
                    <label for="device-id" class="block text-sm font-medium text-gray-700 mb-1">设备编号</label>
                    <input type="text" id="device-id" class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-primary focus:border-transparent" required>
                </div>
                <div class="mb-4">
                    <label for="device-status" class="block text-sm font-medium text-gray-700 mb-1">状态</label>
                    <select id="device-status" class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-primary focus:border-transparent">
                        <option value="available">可用</option>
                        <option value="borrowed">已借出</option>
                        <option value="error">异常</option>
                    </select>
                </div>
                <div class="mb-4">
                    <label for="device-location" class="block text-sm font-medium text-gray-700 mb-1">位置</label>
                    <input type="text" id="device-location" class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-primary focus:border-transparent" value="办公室" required>
                </div>
                <div class="flex justify-end mt-6">
                    <button type="button" id="cancel-add-device-btn" class="px-4 py-2 border border-gray-300 text-gray-700 rounded-lg mr-2 hover:bg-gray-50">取消</button>
                    <button type="submit" class="px-4 py-2 bg-primary text-white rounded-lg hover:bg-primary/90">保存</button>
                </div>
            </form>
        </div>
    </div>

    <!-- 通知提示 -->
    <div id="notification" class="fixed top-4 right-4 z-50 px-6 py-3 rounded-lg shadow-lg transform transition-all duration-300 translate-x-full">
        <div class="flex items-center gap-3">
            <i id="notification-icon" class="fa fa-info-circle"></i>
            <span id="notification-message"></span>
        </div>
    </div>

    <!-- 详情弹窗 -->
    <div id="details-modal" class="fixed inset-0 bg-black bg-opacity-50 z-50 flex items-center justify-center hidden">
        <div class="bg-white rounded-xl shadow-lg w-full max-w-2xl max-h-[80vh] overflow-y-auto">
            <div class="p-6 border-b border-gray-200">
                <div class="flex justify-between items-center">
                    <h3 id="modal-title" class="text-xl font-bold text-gray-900">详情</h3>
                    <button id="close-modal" class="text-gray-500 hover:text-gray-700">
                        <i class="fa fa-times"></i>
                    </button>
                </div>
            </div>
            <div class="p-6">
                <div id="modal-content">
                    <!-- 弹窗内容将通过 JavaScript 动态生成 -->
                </div>
            </div>
            <div class="p-6 border-t border-gray-200 flex justify-end">
                <button id="modal-close-btn" class="px-4 py-2 bg-gray-200 text-gray-700 rounded-lg hover:bg-gray-300">关闭</button>
            </div>
        </div>
    </div>

    <script>
        // 全局变量
        let devices = [];
        let guides = [];
        let records = [];

        // 初始化数据
        function initializeData() {
            // 检查本地存储中是否已有数据
            if (!localStorage.getItem('devices')) {
                // 初始化15个设备
                devices = [];
                for (let i = 1; i <= 15; i++) {
                    devices.push({
                        id: i.toString(),
                        status: i <= 10 ? 'available' : (i <= 14 ? 'borrowed' : 'error'),
                        location: i <= 10 ? '办公室' : (i <= 14 ? '使用中' : '维修'),
                        borrower: i <= 10 ? null : (i <= 14 ? '1' : null),
                        borrowTime: i <= 10 ? null : (i <= 14 ? new Date().toISOString() : null)
                    });
                }
                localStorage.setItem('devices', JSON.stringify(devices));
            } else {
                devices = JSON.parse(localStorage.getItem('devices'));
            }

            if (!localStorage.getItem('guides')) {
                // 初始化导游数据
                guides = [
                    { id: '1', name: '导游A', phone: '13800000001' },
                    { id: '2', name: '导游B', phone: '13800000002' },
                    { id: '3', name: '导游C', phone: '13800000003' }
                ];
                localStorage.setItem('guides', JSON.stringify(guides));
            } else {
                guides = JSON.parse(localStorage.getItem('guides'));
            }

            if (!localStorage.getItem('records')) {
                // 初始化历史记录
                records = [];
                localStorage.setItem('records', JSON.stringify(records));
            } else {
                records = JSON.parse(localStorage.getItem('records'));
            }
        }

        // 保存数据到本地存储
        function saveData() {
            localStorage.setItem('devices', JSON.stringify(devices));
            localStorage.setItem('guides', JSON.stringify(guides));
            localStorage.setItem('records', JSON.stringify(records));
        }

        // 导出数据
        function exportData() {
            try {
                const exportData = {
                    version: '1.0',
                    exportTime: new Date().toISOString(),
                    data: {
                        devices: devices,
                        guides: guides,
                        records: records
                    },
                    metadata: {
                        totalDevices: devices.length,
                        totalGuides: guides.length,
                        totalRecords: records.length
                    }
                };

                const dataStr = JSON.stringify(exportData, null, 2);
                const dataBlob = new Blob([dataStr], { type: 'application/json' });
                const url = URL.createObjectURL(dataBlob);

                const link = document.createElement('a');
                link.href = url;
                link.download = `导览器数据_${new Date().toISOString().split('T')[0]}.json`;
                document.body.appendChild(link);
                link.click();
                document.body.removeChild(link);
                URL.revokeObjectURL(url);

                showNotification('数据导出成功', 'success');
            } catch (error) {
                console.error('导出数据失败:', error);
                showNotification('数据导出失败', 'error');
            }
        }

        // 导入数据
        function importData(file) {
            const reader = new FileReader();
            reader.onload = function(e) {
                try {
                    const importData = JSON.parse(e.target.result);
                    
                    // 验证数据格式
                    if (!importData.version || !importData.data || !importData.data.devices) {
                        showNotification('数据文件格式不正确', 'error');
                        return;
                    }

                    // 确认导入
                    if (confirm(`确定要导入数据吗？\n将导入 ${importData.metadata.totalDevices} 台设备，${importData.metadata.totalGuides} 名导游，${importData.metadata.totalRecords} 条记录。\n此操作将覆盖当前数据。`)) {
                        // 更新数据
                        devices = importData.data.devices;
                        guides = importData.data.guides;
                        records = importData.data.records;

                        // 保存到本地存储
                        saveData();

                        // 重新渲染所有页面
                        renderDashboard();
                        renderDevicesPage();
                        renderGuidesPage();
                        renderBorrowPage();
                        renderReportsPage();
                        renderDataPage();

                        showNotification('数据导入成功', 'success');
                    }
                } catch (error) {
                    console.error('导入数据失败:', error);
                    showNotification('数据导入失败，请检查文件格式', 'error');
                }
            };
            reader.readAsText(file);
        }

        // 渲染数据管理页面
        function renderDataPage() {
            document.getElementById('data-total-devices').textContent = devices.length;
            document.getElementById('data-total-guides').textContent = guides.length;
            document.getElementById('data-total-records').textContent = records.length;
        }

        // 初始化数据管理功能
        function initializeDataManagement() {
            // 导出数据按钮
            document.getElementById('export-data-btn').addEventListener('click', function() {
                exportData();
            });

            // 导入数据按钮
            document.getElementById('import-data-btn').addEventListener('click', function() {
                document.getElementById('import-data-file').click();
            });

            // 文件选择事件
            document.getElementById('import-data-file').addEventListener('change', function(e) {
                const file = e.target.files[0];
                if (file) {
                    importData(file);
                }
            });
        }

        // 显示通知
        function showNotification(message, type = 'info') {
            const notification = document.getElementById('notification');
            const icon = document.getElementById('notification-icon');
            const messageEl = document.getElementById('notification-message');

            messageEl.textContent = message;
            
            // 设置图标和颜色
            switch (type) {
                case 'success':
                    icon.className = 'fa fa-check-circle text-green-500';
                    notification.className = 'fixed top-4 right-4 z-50 px-6 py-3 rounded-lg shadow-lg transform transition-all duration-300 bg-green-50 border border-green-200';
                    break;
                case 'error':
                    icon.className = 'fa fa-exclamation-circle text-red-500';
                    notification.className = 'fixed top-4 right-4 z-50 px-6 py-3 rounded-lg shadow-lg transform transition-all duration-300 bg-red-50 border border-red-200';
                    break;
                case 'warning':
                    icon.className = 'fa fa-exclamation-triangle text-yellow-500';
                    notification.className = 'fixed top-4 right-4 z-50 px-6 py-3 rounded-lg shadow-lg transform transition-all duration-300 bg-yellow-50 border border-yellow-200';
                    break;
                default:
                    icon.className = 'fa fa-info-circle text-blue-500';
                    notification.className = 'fixed top-4 right-4 z-50 px-6 py-3 rounded-lg shadow-lg transform transition-all duration-300 bg-blue-50 border border-blue-200';
            }

            // 显示通知
            notification.classList.remove('translate-x-full');

            // 3秒后隐藏
            setTimeout(() => {
                notification.classList.add('translate-x-full');
            }, 3000);
        }

        // 显示详情弹窗
        function showStatDetails(type) {
            const modal = document.getElementById('details-modal');
            const modalTitle = document.getElementById('modal-title');
            const modalContent = document.getElementById('modal-content');

            let filteredDevices = [];
            let title = '';

            switch (type) {
                case 'total':
                    filteredDevices = devices;
                    title = '所有设备';
                    break;
                case 'available':
                    filteredDevices = devices.filter(device => device.status === 'available');
                    title = '可用设备';
                    break;
                case 'borrowed':
                    filteredDevices = devices.filter(device => device.status === 'borrowed');
                    title = '已借出设备';
                    break;
                case 'error':
                    filteredDevices = devices.filter(device => device.status === 'error');
                    title = '异常设备';
                    break;
            }

            modalTitle.textContent = title;

            let content = `
                <div class="mb-4">
                    <p class="text-gray-500 mb-2">共 ${filteredDevices.length} 台设备</p>
                </div>
                <div class="space-y-3">
            `;

            filteredDevices.forEach(device => {
                const statusText = device.status === 'available' ? '可用' : device.status === 'borrowed' ? '已借出' : '异常';
                const statusClass = device.status === 'available' ? 'text-available' : device.status === 'borrowed' ? 'text-borrowed' : 'text-error';
                const borrowerName = device.borrower ? guides.find(g => g.id === device.borrower)?.name : '无';
                
                content += `
                    <div class="flex justify-between items-center p-3 bg-gray-50 rounded-lg">
                        <div>
                            <p class="font-medium">设备 ${device.id}</p>
                            <p class="text-sm text-gray-500">位置: ${device.location}</p>
                            ${device.borrower ? `<p class="text-sm text-gray-500">使用人: ${borrowerName}</p>` : ''}
                        </div>
                        <span class="px-2 py-1 rounded-full text-xs font-medium ${statusClass} bg-gray-200">${statusText}</span>
                    </div>
                `;
            });

            content += '</div>';
            modalContent.innerHTML = content;

            modal.classList.remove('hidden');
        }

        // 渲染仪表盘
        function renderDashboard() {
            const totalDevices = devices.length;
            const availableDevices = devices.filter(device => device.status === 'available').length;
            const borrowedDevices = devices.filter(device => device.status === 'borrowed').length;
            const errorDevices = devices.filter(device => device.status === 'error').length;

            // 更新统计数字
            document.getElementById('total-devices').textContent = totalDevices;
            document.getElementById('available-devices').textContent = availableDevices;
            document.getElementById('borrowed-devices').textContent = borrowedDevices;
            document.getElementById('error-devices').textContent = errorDevices;

            // 更新侧边栏
            document.getElementById('sidebar-total-devices').textContent = totalDevices;
            document.getElementById('sidebar-available-devices').textContent = availableDevices;

            // 渲染设备卡片
            const deviceCardsContainer = document.getElementById('device-cards-container');
            let cardsHTML = '';

            devices.forEach(device => {
                const statusText = device.status === 'available' ? '可用' : device.status === 'borrowed' ? '已借出' : '异常';
                const statusClass = device.status === 'available' ? 'bg-available/10 text-available' : device.status === 'borrowed' ? 'bg-borrowed/10 text-borrowed' : 'bg-error/10 text-error';
                
                cardsHTML += `
                    <div class="bg-white rounded-lg shadow p-4 cursor-pointer hover:shadow-md transition-all duration-200" onclick="showDeviceDetails('${device.id}')">
                        <div class="flex justify-between items-center mb-2">
                            <h4 class="font-medium">设备 ${device.id}</h4>
                            <span class="px-2 py-1 rounded-full text-xs font-medium ${statusClass}">${statusText}</span>
                        </div>
                        <p class="text-sm text-gray-500">位置: ${device.location}</p>
                        ${device.borrower ? `<p class="text-sm text-gray-500">使用人: ${guides.find(g => g.id === device.borrower)?.name}</p>` : ''}
                    </div>
                `;
            });

            deviceCardsContainer.innerHTML = cardsHTML;

            // 渲染最近活动
            renderRecentActivities();
        }

        // 渲染最近活动
        function renderRecentActivities() {
            const recentActivitiesContainer = document.getElementById('recent-activities');
            const recentRecords = records.slice(-5).reverse();

            if (recentRecords.length === 0) {
                recentActivitiesContainer.innerHTML = `
                    <div class="text-center py-8 text-gray-500">
                        <i class="fa fa-history text-4xl mb-2"></i>
                        <p>暂无活动记录</p>
                    </div>
                `;
                return;
            }

            let activitiesHTML = '';
            recentRecords.forEach(record => {
                const guideName = guides.find(g => g.id === record.guideId)?.name || '未知导游';
                const actionText = record.action === 'borrow' ? '借用了' : '归还了';
                const actionClass = record.action === 'borrow' ? 'text-borrowed' : 'text-available';
                const actionIcon = record.action === 'borrow' ? 'fa-exchange' : 'fa-check-circle';
                
                activitiesHTML += `
                    <div class="flex items-start gap-4 p-3 bg-gray-50 rounded-lg">
                        <div class="w-10 h-10 rounded-full bg-primary/10 flex items-center justify-center text-primary">
                            <i class="fa ${actionIcon}"></i>
                        </div>
                        <div class="flex-1">
                            <p class="font-medium">${guideName} ${actionText} 设备 ${record.deviceId}</p>
                            <p class="text-sm text-gray-500">${new Date(record.timestamp).toLocaleString('zh-CN')}</p>
                            ${record.notes ? `<p class="text-sm text-gray-600 mt-1">备注: ${record.notes}</p>` : ''}
                        </div>
                        <span class="px-2 py-1 rounded-full text-xs font-medium ${actionClass} bg-gray-200">${record.action === 'borrow' ? '借用' : '归还'}</span>
                    </div>
                `;
            });

            recentActivitiesContainer.innerHTML = activitiesHTML;
        }

        // 渲染设备管理页面
        function renderDevicesPage() {
            const devicesTableBody = document.getElementById('devices-table-body');
            const searchTerm = document.getElementById('device-search')?.value.toLowerCase() || '';

            let filteredDevices = devices;
            if (searchTerm) {
                filteredDevices = devices.filter(device => 
                    device.id.toLowerCase().includes(searchTerm) ||
                    device.location.toLowerCase().includes(searchTerm)
                );
            }

            let tableHTML = '';
            filteredDevices.forEach(device => {
                const statusText = device.status === 'available' ? '可用' : device.status === 'borrowed' ? '已借出' : '异常';
                const statusClass = device.status === 'available' ? 'text-available' : device.status === 'borrowed' ? 'text-borrowed' : 'text-error';
                const borrowerName = device.borrower ? guides.find(g => g.id === device.borrower)?.name : '-';
                const borrowTime = device.borrowTime ? new Date(device.borrowTime).toLocaleString('zh-CN') : '-';
                
                tableHTML += `
                    <tr class="hover:bg-gray-50">
                        <td class="px-6 py-4 whitespace-nowrap">${device.id}</td>
                        <td class="px-6 py-4 whitespace-nowrap">
                            <span class="px-2 py-1 rounded-full text-xs font-medium ${statusClass} bg-gray-200">${statusText}</span>
                        </td>
                        <td class="px-6 py-4 whitespace-nowrap">${device.location}</td>
                        <td class="px-6 py-4 whitespace-nowrap">${borrowerName}</td>
                        <td class="px-6 py-4 whitespace-nowrap text-gray-500">${borrowTime}</td>
                        <td class="px-6 py-4 whitespace-nowrap text-sm">
                            <button class="text-primary hover:text-primary/80 mr-3" onclick="editDevice('${device.id}')">编辑</button>
                            <button class="text-error hover:text-error/80" onclick="deleteDevice('${device.id}')">删除</button>
                        </td>
                    </tr>
                `;
            });

            devicesTableBody.innerHTML = tableHTML;
        }

        // 渲染导游管理页面
        function renderGuidesPage() {
            const guidesTableBody = document.getElementById('guides-table-body');
            
            let tableHTML = '';
            guides.forEach(guide => {
                const activeDevices = devices.filter(device => device.borrower === guide.id).length;
                const statusText = activeDevices > 0 ? '使用中' : '空闲';
                const statusClass = activeDevices > 0 ? 'text-borrowed' : 'text-gray-500';
                
                tableHTML += `
                    <tr class="hover:bg-gray-50">
                        <td class="px-6 py-4 whitespace-nowrap">${guide.id}</td>
                        <td class="px-6 py-4 whitespace-nowrap font-medium">${guide.name}</td>
                        <td class="px-6 py-4 whitespace-nowrap text-gray-500">${guide.phone || '-'}</td>
                        <td class="px-6 py-4 whitespace-nowrap">
                            <span class="px-2 py-1 rounded-full text-xs font-medium ${statusClass} bg-gray-200">${statusText}</span>
                        </td>
                        <td class="px-6 py-4 whitespace-nowrap text-sm">
                            <button class="text-primary hover:text-primary/80 mr-3" onclick="editGuide('${guide.id}')">编辑</button>
                            <button class="text-error hover:text-error/80" onclick="deleteGuide('${guide.id}')">删除</button>
                        </td>
                    </tr>
                `;
            });

            guidesTableBody.innerHTML = tableHTML;
        }

        // 渲染借用归还页面
        function renderBorrowPage() {
            // 导游选择器
            const guideSelect = document.getElementById('guide-select');
            let guideOptions = '<option value="">请选择导游</option>';
            guides.forEach(guide => {
                guideOptions += `<option value="${guide.id}">${guide.name}</option>`;
            });
            guideSelect.innerHTML = guideOptions;

            // 设备选择器（借用）
            const deviceSelect = document.getElementById('device-select');
            let deviceOptions = '<option value="">请选择设备</option>';
            devices.filter(device => device.status === 'available').forEach(device => {
                deviceOptions += `<option value="${device.id}">设备 ${device.id}</option>`;
            });
            deviceSelect.innerHTML = deviceOptions;

            // 设备选择器（归还）
            const returnDeviceSelect = document.getElementById('return-device-select');
            let returnDeviceOptions = '<option value="">请选择设备</option>';
            devices.filter(device => device.status === 'borrowed').forEach(device => {
                const guideName = guides.find(g => g.id === device.borrower)?.name || '未知';
                returnDeviceOptions += `<option value="${device.id}">设备 ${device.id} (${guideName})</option>`;
            });
            returnDeviceSelect.innerHTML = returnDeviceOptions;
        }

        // 渲染统计报表页面
        function renderReportsPage() {
            // 设备状态统计图表
            const statusCtx = document.getElementById('device-status-chart').getContext('2d');
            const statusChart = new Chart(statusCtx, {
                type: 'doughnut',
                data: {
                    labels: ['可用', '已借出', '异常'],
                    datasets: [{
                        data: [
                            devices.filter(d => d.status === 'available').length,
                            devices.filter(d => d.status === 'borrowed').length,
                            devices.filter(d => d.status === 'error').length
                        ],
                        backgroundColor: ['#10b981', '#f59e0b', '#ef4444'],
                        borderWidth: 0
                    }]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    plugins: {
                        legend: {
                            position: 'bottom'
                        }
                    }
                }
            });

            // 导游使用统计图表
            const guideUsage = {};
            devices.filter(d => d.status === 'borrowed').forEach(d => {
                if (d.borrower) {
                    guideUsage[d.borrower] = (guideUsage[d.borrower] || 0) + 1;
                }
            });

            const guideLabels = [];
            const guideData = [];
            const guideColors = ['#3b82f6', '#8b5cf6', '#ec4899', '#f43f5e', '#f97316'];

            guides.forEach((guide, index) => {
                guideLabels.push(guide.name);
                guideData.push(guideUsage[guide.id] || 0);
            });

            const guideCtx = document.getElementById('guide-usage-chart').getContext('2d');
            const guideChart = new Chart(guideCtx, {
                type: 'bar',
                data: {
                    labels: guideLabels,
                    datasets: [{
                        label: '使用设备数',
                        data: guideData,
                        backgroundColor: guideColors.slice(0, guideLabels.length),
                        borderWidth: 0,
                        borderRadius: 4
                    }]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    plugins: {
                        legend: {
                            display: false
                        }
                    },
                    scales: {
                        y: {
                            beginAtZero: true,
                            ticks: {
                                stepSize: 1
                            }
                        }
                    }
                }
            });
        }

        // 页面切换
        function switchPage(pageName) {
            // 更新导航栏激活状态
            document.querySelectorAll('.nav-item').forEach(item => {
                item.classList.remove('active');
                if (item.dataset.page === pageName) {
                    item.classList.add('active');
                }
            });

            // 更新页面标题
            const pageTitles = {
                'dashboard': '仪表盘',
                'devices': '设备管理',
                'borrow': '借用归还',
                'reports': '统计报表',
                'guides': '导游管理'
            };
            document.getElementById('page-title').textContent = pageTitles[pageName];

            // 显示对应页面
            document.querySelectorAll('.page').forEach(page => {
                page.classList.add('hidden');
            });
            document.getElementById(`${pageName}-page`).classList.remove('hidden');

            // 根据页面类型执行不同的渲染函数
            switch (pageName) {
                case 'dashboard':
                    renderDashboard();
                    break;
                case 'devices':
                    renderDevicesPage();
                    break;
                case 'borrow':
                    renderBorrowPage();
                    break;
                case 'reports':
                    renderReportsPage();
                    break;
                case 'guides':
                    renderGuidesPage();
                    break;
            }

            // 关闭移动端菜单
            document.getElementById('mobile-menu').classList.add('translate-x-full');
        }

        // 事件监听器
        document.addEventListener('DOMContentLoaded', function() {
            // 初始化数据
            initializeData();

            // 初始化数据管理功能
            initializeDataManagement();

            // 设置当前日期
            const now = new Date();
            const dateOptions = { year: 'numeric', month: 'long', day: 'numeric', weekday: 'long' };
            document.getElementById('current-date').textContent = now.toLocaleDateString('zh-CN', dateOptions);

            // 导航栏点击事件
            document.querySelectorAll('.nav-item').forEach(item => {
                item.addEventListener('click', function(e) {
                    e.preventDefault();
                    const pageName = this.dataset.page;
                    switchPage(pageName);
                });
            });

            // 移动端菜单
            document.getElementById('mobile-menu-btn').addEventListener('click', function() {
                document.getElementById('mobile-menu').classList.remove('translate-x-full');
            });

            document.getElementById('close-mobile-menu').addEventListener('click', function() {
                document.getElementById('mobile-menu').classList.add('translate-x-full');
            });

            // 关闭弹窗
            document.getElementById('close-modal').addEventListener('click', function() {
                document.getElementById('details-modal').classList.add('hidden');
            });

            document.getElementById('modal-close-btn').addEventListener('click', function() {
                document.getElementById('details-modal').classList.add('hidden');
            });

            // 借用表单提交
            document.getElementById('borrow-form').addEventListener('submit', function(e) {
                e.preventDefault();
                const guideId = document.getElementById('guide-select').value;
                const deviceId = document.getElementById('device-select').value;
                const notes = document.getElementById('borrow-notes').value;

                if (!guideId || !deviceId) {
                    showNotification('请选择导游和设备', 'warning');
                    return;
                }

                const device = devices.find(d => d.id === deviceId);
                if (!device || device.status !== 'available') {
                    showNotification('设备不可用', 'error');
                    return;
                }

                // 更新设备状态
                device.status = 'borrowed';
                device.location = '使用中';
                device.borrower = guideId;
                device.borrowTime = new Date().toISOString();

                // 记录历史
                records.push({
                    id: Date.now().toString(),
                    guideId,
                    deviceId,
                    action: 'borrow',
                    timestamp: new Date().toISOString(),
                    notes
                });

                saveData();
                showNotification('设备借用成功', 'success');
                renderBorrowPage();
                renderDashboard();
                this.reset();
            });

            // 归还表单提交
            document.getElementById('return-form').addEventListener('submit', function(e) {
                e.preventDefault();
                const deviceId = document.getElementById('return-device-select').value;
                const location = document.getElementById('return-location').value;
                const notes = document.getElementById('return-notes').value;

                if (!deviceId || !location) {
                    showNotification('请选择设备和归还位置', 'warning');
                    return;
                }

                const device = devices.find(d => d.id === deviceId);
                if (!device || device.status !== 'borrowed') {
                    showNotification('设备状态错误', 'error');
                    return;
                }

                const guideId = device.borrower;

                // 更新设备状态
                device.status = 'available';
                device.location = location;
                device.borrower = null;
                device.borrowTime = null;

                // 记录历史
                records.push({
                    id: Date.now().toString(),
                    guideId,
                    deviceId,
                    action: 'return',
                    timestamp: new Date().toISOString(),
                    notes
                });

                saveData();
                showNotification('设备归还成功', 'success');
                renderBorrowPage();
                renderDashboard();
                this.reset();
            });

            // 添加导游按钮
            document.getElementById('add-guide-btn').addEventListener('click', function() {
                document.getElementById('add-guide-modal').classList.remove('hidden');
            });

            // 关闭导游模态框
            document.getElementById('close-guide-modal').addEventListener('click', function() {
                document.getElementById('add-guide-modal').classList.add('hidden');
            });

            document.getElementById('cancel-guide-btn').addEventListener('click', function() {
                document.getElementById('add-guide-modal').classList.add('hidden');
            });

            // 导游表单提交
            document.getElementById('guide-form').addEventListener('submit', function(e) {
                e.preventDefault();
                const name = document.getElementById('guide-name').value;
                const phone = document.getElementById('guide-phone').value;

                if (!name) {
                    showNotification('请输入导游姓名', 'warning');
                    return;
                }

                const newGuide = {
                    id: Date.now().toString(),
                    name,
                    phone
                };

                guides.push(newGuide);
                saveData();
                showNotification('导游添加成功', 'success');
                renderGuidesPage();
                renderBorrowPage();
                this.reset();
                document.getElementById('add-guide-modal').classList.add('hidden');
            });

            // 添加设备按钮
            document.getElementById('add-device-btn').addEventListener('click', function() {
                document.getElementById('add-device-modal').classList.remove('hidden');
            });

            // 关闭设备模态框
            document.getElementById('close-add-device-modal').addEventListener('click', function() {
                document.getElementById('add-device-modal').classList.add('hidden');
            });

            document.getElementById('cancel-add-device-btn').addEventListener('click', function() {
                document.getElementById('add-device-modal').classList.add('hidden');
            });

            // 设备表单提交
            document.getElementById('add-device-form').addEventListener('submit', function(e) {
                e.preventDefault();
                const id = document.getElementById('device-id').value;
                const status = document.getElementById('device-status').value;
                const location = document.getElementById('device-location').value;

                if (!id || !location) {
                    showNotification('请输入设备编号和位置', 'warning');
                    return;
                }

                if (devices.find(d => d.id === id)) {
                    showNotification('设备编号已存在', 'error');
                    return;
                }

                const newDevice = {
                    id,
                    status,
                    location,
                    borrower: null,
                    borrowTime: null
                };

                devices.push(newDevice);
                saveData();
                showNotification('设备添加成功', 'success');
                renderDevicesPage();
                renderDashboard();
                this.reset();
                document.getElementById('add-device-modal').classList.add('hidden');
            });

            // 设备搜索
            document.getElementById('device-search')?.addEventListener('input', function() {
                renderDevicesPage();
            });

            // 仪表盘刷新按钮
            document.getElementById('dashboard-refresh-btn')?.addEventListener('click', function() {
                renderDashboard();
                showNotification('数据已刷新', 'success');
            });

            // 初始渲染
            renderDashboard();
        });

        // 编辑设备
        function editDevice(deviceId) {
            const device = devices.find(d => d.id === deviceId);
            if (!device) return;

            const newLocation = prompt('请输入新的位置:', device.location);
            if (newLocation !== null) {
                device.location = newLocation;
                saveData();
                renderDevicesPage();
                renderDashboard();
                showNotification('设备信息已更新', 'success');
            }
        }

        // 删除设备
        function deleteDevice(deviceId) {
            if (!confirm('确定要删除这个设备吗？')) return;

            const device = devices.find(d => d.id === deviceId);
            if (device && device.status === 'borrowed') {
                showNotification('设备正在使用中，无法删除', 'error');
                return;
            }

            devices = devices.filter(d => d.id !== deviceId);
            saveData();
            renderDevicesPage();
            renderDashboard();
            showNotification('设备已删除', 'success');
        }

        // 编辑导游
        function editGuide(guideId) {
            const guide = guides.find(g => g.id === guideId);
            if (!guide) return;

            const newName = prompt('请输入新的姓名:', guide.name);
            if (newName !== null) {
                guide.name = newName;
                const newPhone = prompt('请输入新的电话:', guide.phone || '');
                if (newPhone !== null) {
                    guide.phone = newPhone;
                }
                saveData();
                renderGuidesPage();
                renderBorrowPage();
                showNotification('导游信息已更新', 'success');
            }
        }

        // 删除导游
        function deleteGuide(guideId) {
            if (!confirm('确定要删除这个导游吗？')) return;

            const activeDevices = devices.filter(d => d.borrower === guideId).length;
            if (activeDevices > 0) {
                showNotification('该导游还有未归还的设备，无法删除', 'error');
                return;
            }

            guides = guides.filter(g => g.id !== guideId);
            saveData();
            renderGuidesPage();
            renderBorrowPage();
            showNotification('导游已删除', 'success');
        }

        // 显示设备详情
        function showDeviceDetails(deviceId) {
            const device = devices.find(d => d.id === deviceId);
            if (!device) return;

            const modal = document.getElementById('details-modal');
            const modalTitle = document.getElementById('modal-title');
            const modalContent = document.getElementById('modal-content');

            modalTitle.textContent = `设备 ${device.id} 详情`;

            const statusText = device.status === 'available' ? '可用' : device.status === 'borrowed' ? '已借出' : '异常';
            const statusClass = device.status === 'available' ? 'text-available' : device.status === 'borrowed' ? 'text-borrowed' : 'text-error';
            const borrowerName = device.borrower ? guides.find(g => g.id === device.borrower)?.name : '无';
            const borrowTime = device.borrowTime ? new Date(device.borrowTime).toLocaleString('zh-CN') : '无';

            let content = `
                <div class="space-y-4">
                    <div class="flex justify-between items-center">
                        <span class="text-gray-500">设备编号</span>
                        <span class="font-medium">${device.id}</span>
                    </div>
                    <div class="flex justify-between items-center">
                        <span class="text-gray-500">状态</span>
                        <span class="px-2 py-1 rounded-full text-xs font-medium ${statusClass} bg-gray-200">${statusText}</span>
                    </div>
                    <div class="flex justify-between items-center">
                        <span class="text-gray-500">位置</span>
                        <span>${device.location}</span>
                    </div>
                    <div class="flex justify-between items-center">
                        <span class="text-gray-500">使用人</span>
                        <span>${borrowerName}</span>
                    </div>
                    <div class="flex justify-between items-center">
                        <span class="text-gray-500">借出时间</span>
                        <span>${borrowTime}</span>
                    </div>
                </div>
            `;

            modalContent.innerHTML = content;
            modal.classList.remove('hidden');
        }
    </script>
</body>
</html>