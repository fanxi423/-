<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>在线作业系统</title>
    <style>
        body {
            font-family: 'Arial', sans-serif;
            line-height: 1.6;
            margin: 0;
            padding: 0;
            background-color: #f5f5f5;
            color: #333;
        }
        .container {
            width: 90%;
            max-width: 1200px;
            margin: 0 auto;
            padding: 20px;
        }
        header {
            background-color: #4285f4;
            color: white;
            padding: 20px 0;
            margin-bottom: 30px;
            box-shadow: 0 2px 5px rgba(0,0,0,0.1);
        }
        header h1 {
            margin: 0;
            text-align: center;
        }
        .auth-section {
            display: flex;
            justify-content: center;
            margin-bottom: 30px;
        }
        .auth-form {
            background: white;
            padding: 20px;
            border-radius: 5px;
            box-shadow: 0 2px 10px rgba(0,0,0,0.1);
            width: 300px;
            margin: 0 10px;
        }
        .auth-form h2 {
            margin-top: 0;
            color: #4285f4;
        }
        input[type="text"],
        input[type="password"],
        input[type="email"],
        select, textarea {
            width: 100%;
            padding: 10px;
            margin: 8px 0;
            border: 1px solid #ddd;
            border-radius: 4px;
            box-sizing: border-box;
        }
        button {
            background-color: #4285f4;
            color: white;
            padding: 10px 15px;
            border: none;
            border-radius: 4px;
            cursor: pointer;
            font-size: 16px;
            width: 100%;
        }
        button:hover {
            background-color: #3367d6;
        }
        .content-section {
            display: none;
            background: white;
            padding: 20px;
            border-radius: 5px;
            box-shadow: 0 2px 10px rgba(0,0,0,0.1);
            margin-bottom: 30px;
        }
        .tab-container {
            display: flex;
            margin-bottom: 20px;
            border-bottom: 1px solid #ddd;
        }
        .tab {
            padding: 10px 20px;
            cursor: pointer;
            background: #f1f1f1;
            margin-right: 5px;
            border-radius: 5px 5px 0 0;
        }
        .tab.active {
            background: #4285f4;
            color: white;
        }
        .tab-content {
            display: none;
        }
        .tab-content.active {
            display: block;
        }
        .assignment-list {
            list-style: none;
            padding: 0;
        }
        .assignment-item {
            padding: 15px;
            border: 1px solid #ddd;
            margin-bottom: 10px;
            border-radius: 5px;
            background: #f9f9f9;
        }
        .assignment-item h3 {
            margin-top: 0;
        }
        .message-system {
            display: flex;
        }
        .message-users {
            width: 30%;
            border-right: 1px solid #ddd;
            padding-right: 10px;
        }
        .message-conversation {
            width: 70%;
            padding-left: 10px;
        }
        .message {
            padding: 8px 12px;
            margin: 5px 0;
            border-radius: 4px;
            max-width: 70%;
        }
        .message.sent {
            background: #e3f2fd;
            margin-left: auto;
        }
        .message.received {
            background: #f1f1f1;
        }
        .file-upload {
            margin: 20px 0;
        }
        .grading-tool {
            border: 1px solid #ddd;
            padding: 15px;
            margin-top: 15px;
            background: #fffde7;
        }
        .reminder {
            background: #fff3e0;
            padding: 10px;
            border-left: 4px solid #ff9800;
            margin: 10px 0;
        }
        .error {
            color: #d32f2f;
            font-size: 14px;
            margin-top: 5px;
        }
    </style>
</head>
<body>
    <header>
        <div class="container">
            <h1>在线作业系统</h1>
        </div>
    </header>

    <div class="container">
        <!-- 登录/注册部分 -->
        <div class="auth-section" id="authSection">
            <div class="auth-form">
                <h2>登录</h2>
                <form id="loginForm">
                    <input type="email" id="loginEmail" placeholder="邮箱" required>
                    <input type="password" id="loginPassword" placeholder="密码" required>
                    <button type="submit">登录</button>
                    <div id="loginError" class="error"></div>
                </form>
            </div>
            <div class="auth-form">
                <h2>注册</h2>
                <form id="registerForm">
                    <input type="text" id="registerName" placeholder="姓名" required>
                    <input type="email" id="registerEmail" placeholder="邮箱" required>
                    <select id="registerRole" required>
                        <option value="">选择角色</option>
                        <option value="student">学生</option>
                        <option value="teacher">教师</option>
                    </select>
                    <input type="password" id="registerPassword" placeholder="密码" required>
                    <input type="password" id="registerConfirmPassword" placeholder="确认密码" required>
                    <button type="submit">注册</button>
                    <div id="registerError" class="error"></div>
                </form>
            </div>
        </div>

        <!-- 主内容区域 (登录后显示) -->
        <div id="mainContent" class="content-section">
            <div class="tab-container">
                <div class="tab active" data-tab="assignments">作业</div>
                <div class="tab" data-tab="messages">消息</div>
                <div class="tab" data-tab="profile">个人资料</div>
                <div class="tab" id="logoutTab">退出</div>
            </div>

            <!-- 作业标签内容 -->
            <div class="tab-content active" id="assignmentsTab">
                <h2>我的作业</h2>
                <div id="assignmentList" class="assignment-list">
                    <!-- 作业列表将通过JavaScript动态生成 -->
                </div>
                
                <!-- 教师专用: 创建作业 -->
                <div id="teacherAssignmentCreate" style="display: none;">
                    <h3>创建新作业</h3>
                    <form id="createAssignmentForm">
                        <input type="text" id="assignmentTitle" placeholder="作业标题" required>
                        <textarea id="assignmentDescription" placeholder="作业描述" rows="4" required></textarea>
                        <input type="datetime-local" id="assignmentDueDate" required>
                        <button type="submit">发布作业</button>
                    </form>
                </div>
                
                <!-- 学生专用: 提交作业 -->
                <div id="studentAssignmentSubmit" style="display: none;">
                    <h3>提交作业</h3>
                    <form id="submitAssignmentForm">
                        <input type="hidden" id="submitAssignmentId">
                        <textarea id="assignmentSubmission" placeholder="你的回答" rows="6" required></textarea>
                        <div class="file-upload">
                            <label for="assignmentFile">上传文件 (可选):</label>
                            <input type="file" id="assignmentFile">
                            <div id="fileValidationError" class="error"></div>
                        </div>
                        <button type="submit">提交</button>
                    </form>
                </div>
                
                <!-- 教师专用: 批改作业 -->
                <div id="teacherGradingSection" style="display: none;">
                    <h3>批改作业</h3>
                    <div class="grading-tool">
                        <h4 id="gradingAssignmentTitle"></h4>
                        <p id="gradingStudentName"></p>
                        <div id="gradingSubmissionContent"></div>
                        <div id="gradingSubmissionFile"></div>
                        
                        <form id="gradingForm">
                            <input type="hidden" id="gradingSubmissionId">
                            <label for="grade">分数:</label>
                            <input type="number" id="grade" min="0" max="100" required>
                            <label for="feedback">反馈:</label>
                            <textarea id="feedback" rows="4" required></textarea>
                            <button type="submit">提交评分</button>
                        </form>
                    </div>
                </div>
                
                <!-- 作业提醒 -->
                <div id="assignmentReminders">
                    <!-- 提醒将通过JavaScript动态生成 -->
                </div>
            </div>

            <!-- 消息标签内容 -->
            <div class="tab-content" id="messagesTab">
                <h2>消息系统</h2>
                <div class="message-system">
                    <div class="message-users">
                        <h3>联系人</h3>
                        <ul id="messageContacts">
                            <!-- 联系人列表将通过JavaScript动态生成 -->
                        </ul>
                    </div>
                    <div class="message-conversation">
                        <h3 id="currentConversation">选择联系人开始聊天</h3>
                        <div id="messageHistory">
                            <!-- 消息历史将通过JavaScript动态生成 -->
                        </div>
                        <form id="sendMessageForm" style="display: none;">
                            <input type="hidden" id="receiverId">
                            <textarea id="messageText" placeholder="输入消息..." rows="3" required></textarea>
                            <button type="submit">发送</button>
                        </form>
                    </div>
                </div>
            </div>

            <!-- 个人资料标签内容 -->
            <div class="tab-content" id="profileTab">
                <h2>个人资料</h2>
                <form id="profileForm">
                    <label for="profileName">姓名:</label>
                    <input type="text" id="profileName" required>
                    
                    <label for="profileEmail">邮箱:</label>
                    <input type="email" id="profileEmail" required disabled>
                    
                    <label for="profileRole">角色:</label>
                    <input type="text" id="profileRole" disabled>
                    
                    <label for="currentPassword">当前密码 (留空则不修改):</label>
                    <input type="password" id="currentPassword">
                    
                    <label for="newPassword">新密码:</label>
                    <input type="password" id="newPassword">
                    
                    <label for="confirmNewPassword">确认新密码:</label>
                    <input type="password" id="confirmNewPassword">
                    
                    <button type="submit">更新资料</button>
                    <div id="profileError" class="error"></div>
                </form>
            </div>
        </div>
    </div>

    <script>
        // 数据库模拟 (使用localStorage)
        const db = {
            // 初始化数据库
            init: function() {
                if (!localStorage.getItem('users')) {
                    localStorage.setItem('users', JSON.stringify([]));
                }
                if (!localStorage.getItem('assignments')) {
                    localStorage.setItem('assignments', JSON.stringify([]));
                }
                if (!localStorage.getItem('submissions')) {
                    localStorage.setItem('submissions', JSON.stringify([]));
                }
                if (!localStorage.getItem('messages')) {
                    localStorage.setItem('messages', JSON.stringify([]));
                }
            },
            
            // 用户相关操作
            getUserByEmail: function(email) {
                const users = JSON.parse(localStorage.getItem('users'));
                return users.find(user => user.email === email);
            },
            
            addUser: function(user) {
                const users = JSON.parse(localStorage.getItem('users'));
                users.push(user);
                localStorage.setItem('users', JSON.stringify(users));
            },
            
            updateUser: function(updatedUser) {
                const users = JSON.parse(localStorage.getItem('users'));
                const index = users.findIndex(u => u.id === updatedUser.id);
                if (index !== -1) {
                    users[index] = updatedUser;
                    localStorage.setItem('users', JSON.stringify(users));
                }
            },
            
            // 作业相关操作
            addAssignment: function(assignment) {
                const assignments = JSON.parse(localStorage.getItem('assignments'));
                assignments.push(assignment);
                localStorage.setItem('assignments', JSON.stringify(assignments));
            },
            
            getAssignments: function() {
                return JSON.parse(localStorage.getItem('assignments'));
            },
            
            getAssignmentById: function(id) {
                const assignments = JSON.parse(localStorage.getItem('assignments'));
                return assignments.find(a => a.id === id);
            },
            
            // 作业提交相关
            addSubmission: function(submission) {
                const submissions = JSON.parse(localStorage.getItem('submissions'));
                submissions.push(submission);
                localStorage.setItem('submissions', JSON.stringify(submissions));
            },
            
            getSubmissionsByAssignment: function(assignmentId) {
                const submissions = JSON.parse(localStorage.getItem('submissions'));
                return submissions.filter(s => s.assignmentId === assignmentId);
            },
            
            getSubmissionById: function(id) {
                const submissions = JSON.parse(localStorage.getItem('submissions'));
                return submissions.find(s => s.id === id);
            },
            
            updateSubmission: function(updatedSubmission) {
                const submissions = JSON.parse(localStorage.getItem('submissions'));
                const index = submissions.findIndex(s => s.id === updatedSubmission.id);
                if (index !== -1) {
                    submissions[index] = updatedSubmission;
                    localStorage.setItem('submissions', JSON.stringify(submissions));
                }
            },
            
            // 消息相关
            addMessage: function(message) {
                const messages = JSON.parse(localStorage.getItem('messages'));
                messages.push(message);
                localStorage.setItem('messages', JSON.stringify(messages));
            },
            
            getMessagesBetweenUsers: function(user1Id, user2Id) {
                const messages = JSON.parse(localStorage.getItem('messages'));
                return messages.filter(m => 
                    (m.senderId === user1Id && m.receiverId === user2Id) || 
                    (m.senderId === user2Id && m.receiverId === user1Id)
                ).sort((a, b) => new Date(a.timestamp) - new Date(b.timestamp));
            },
            
            getUsers: function() {
                return JSON.parse(localStorage.getItem('users'));
            },
            
            // 生成ID
            generateId: function() {
                return Date.now().toString(36) + Math.random().toString(36).substr(2);
            }
        };

        // 应用状态管理
        const app = {
            currentUser: null,
            
            init: function() {
                db.init();
                this.bindEvents();
                this.checkAuth();
            },
            
            bindEvents: function() {
                // 登录/注册表单
                document.getElementById('loginForm').addEventListener('submit', this.handleLogin.bind(this));
                document.getElementById('registerForm').addEventListener('submit', this.handleRegister.bind(this));
                
                // 标签切换
                document.querySelectorAll('.tab').forEach(tab => {
                    tab.addEventListener('click', this.handleTabSwitch.bind(this));
                });
                
                // 退出
                document.getElementById('logoutTab').addEventListener('click', this.handleLogout.bind(this));
                
                // 作业相关
                document.getElementById('createAssignmentForm')?.addEventListener('submit', this.handleCreateAssignment.bind(this));
                document.getElementById('submitAssignmentForm')?.addEventListener('submit', this.handleSubmitAssignment.bind(this));
                document.getElementById('gradingForm')?.addEventListener('submit', this.handleGradeSubmission.bind(this));
                
                // 消息相关
                document.getElementById('sendMessageForm')?.addEventListener('submit', this.handleSendMessage.bind(this));
                
                // 个人资料
                document.getElementById('profileForm')?.addEventListener('submit', this.handleUpdateProfile.bind(this));
                
                // 文件上传验证
                document.getElementById('assignmentFile')?.addEventListener('change', this.validateFileUpload.bind(this));
            },
            
            checkAuth: function() {
                const userData = localStorage.getItem('currentUser');
                if (userData) {
                    this.currentUser = JSON.parse(userData);
                    this.showMainContent();
                } else {
                    this.showAuthSection();
                }
            },
            
            showAuthSection: function() {
                document.getElementById('authSection').style.display = 'flex';
                document.getElementById('mainContent').style.display = 'none';
            },
            
            showMainContent: function() {
                document.getElementById('authSection').style.display = 'none';
                document.getElementById('mainContent').style.display = 'block';
                
                // 根据用户角色显示不同内容
                if (this.currentUser.role === 'teacher') {
                    document.getElementById('teacherAssignmentCreate').style.display = 'block';
                    document.getElementById('teacherGradingSection').style.display = 'block';
                    document.getElementById('studentAssignmentSubmit').style.display = 'none';
                } else {
                    document.getElementById('teacherAssignmentCreate').style.display = 'none';
                    document.getElementById('teacherGradingSection').style.display = 'none';
                    document.getElementById('studentAssignmentSubmit').style.display = 'block';
                }
                
                // 加载用户特定数据
                this.loadAssignments();
                this.loadReminders();
                this.loadMessageContacts();
                this.loadProfileData();
            },
            
            // 认证处理
            handleLogin: function(e) {
                e.preventDefault();
                const email = document.getElementById('loginEmail').value;
                const password = document.getElementById('loginPassword').value;
                
                const user = db.getUserByEmail(email);
                if (!user || user.password !== password) {
                    document.getElementById('loginError').textContent = '无效的邮箱或密码';
                    return;
                }
                
                this.currentUser = user;
                localStorage.setItem('currentUser', JSON.stringify(user));
                this.showMainContent();
            },
            
            handleRegister: function(e) {
                e.preventDefault();
                const name = document.getElementById('registerName').value;
                const email = document.getElementById('registerEmail').value;
                const role = document.getElementById('registerRole').value;
                const password = document.getElementById('registerPassword').value;
                const confirmPassword = document.getElementById('registerConfirmPassword').value;
                
                if (password !== confirmPassword) {
                    document.getElementById('registerError').textContent = '密码不匹配';
                    return;
                }
                
                if (db.getUserByEmail(email)) {
                    document.getElementById('registerError').textContent = '该邮箱已被注册';
                    return;
                }
                
                const newUser = {
                    id: db.generateId(),
                    name,
                    email,
                    role,
                    password,
                    createdAt: new Date().toISOString()
                };
                
                db.addUser(newUser);
                this.currentUser = newUser;
                localStorage.setItem('currentUser', JSON.stringify(newUser));
                this.showMainContent();
            },
            
            handleLogout: function() {
                localStorage.removeItem('currentUser');
                this.currentUser = null;
                this.showAuthSection();
                
                // 重置表单
                document.getElementById('loginForm').reset();
                document.getElementById('registerForm').reset();
                document.getElementById('loginError').textContent = '';
                document.getElementById('registerError').textContent = '';
            },
            
            // 标签切换
            handleTabSwitch: function(e) {
                const tabId = e.target.getAttribute('data-tab');
                if (!tabId) return; // 退出按钮没有data-tab属性
                
                // 更新活动标签
                document.querySelectorAll('.tab').forEach(tab => {
                    tab.classList.remove('active');
                });
                e.target.classList.add('active');
                
                // 显示对应内容
                document.querySelectorAll('.tab-content').forEach(content => {
                    content.classList.remove('active');
                });
                document.getElementById(`${tabId}Tab`).classList.add('active');
                
                // 加载特定标签的数据
                if (tabId === 'messages') {
                    this.loadMessageContacts();
                }
            },
            
            // 作业相关
            loadAssignments: function() {
                const assignments = db.getAssignments();
                const assignmentList = document.getElementById('assignmentList');
                assignmentList.innerHTML = '';
                
                if (this.currentUser.role === 'teacher') {
                    // 教师视图 - 显示所有作业和他们创建的作业
                    assignments.forEach(assignment => {
                        const assignmentItem = this.createAssignmentElement(assignment);
                        assignmentList.appendChild(assignmentItem);
                    });
                } else {
                    // 学生视图 - 显示所有作业和提交状态
                    const submissions = db.getSubmissionsByAssignment();
                    
                    assignments.forEach(assignment => {
                        const assignmentItem = this.createAssignmentElement(assignment);
                        
                        // 检查学生是否已提交
                        const studentSubmission = submissions.find(s => 
                            s.assignmentId === assignment.id && s.studentId === this.currentUser.id
                        );
                        
                        if (studentSubmission) {
                            const statusDiv = document.createElement('div');
                            statusDiv.textContent = `已提交 - ${studentSubmission.graded ? `已评分: ${studentSubmission.grade}` : '待评分'}`;
                            statusDiv.style.color = studentSubmission.graded ? 'green' : 'orange';
                            assignmentItem.appendChild(statusDiv);
                            
                            if (studentSubmission.feedback) {
                                const feedbackDiv = document.createElement('div');
                                feedbackDiv.textContent = `教师反馈: ${studentSubmission.feedback}`;
                                assignmentItem.appendChild(feedbackDiv);
                            }
                        }
                        
                        assignmentList.appendChild(assignmentItem);
                    });
                }
            },
            
            createAssignmentElement: function(assignment) {
                const assignmentItem = document.createElement('li');
                assignmentItem.className = 'assignment-item';
                
                const title = document.createElement('h3');
                title.textContent = assignment.title;
                
                const description = document.createElement('p');
                description.textContent = assignment.description;
                
                const dueDate = document.createElement('p');
                dueDate.textContent = `截止日期: ${new Date(assignment.dueDate).toLocaleString()}`;
                
                assignmentItem.appendChild(title);
                assignmentItem.appendChild(description);
                assignmentItem.appendChild(dueDate);
                
                // 添加操作按钮
                if (this.currentUser.role === 'teacher' && assignment.teacherId === this.currentUser.id) {
                    // 教师可以查看提交
                    const viewSubmissionsBtn = document.createElement('button');
                    viewSubmissionsBtn.textContent = '查看提交';
                    viewSubmissionsBtn.addEventListener('click', () => this.viewSubmissions(assignment.id));
                    assignmentItem.appendChild(viewSubmissionsBtn);
                } else if (this.currentUser.role === 'student') {
                    // 学生可以提交作业
                    const submitBtn = document.createElement('button');
                    submitBtn.textContent = '提交作业';
                    submitBtn.addEventListener('click', () => this.prepareSubmissionForm(assignment.id));
                    assignmentItem.appendChild(submitBtn);
                }
                
                return assignmentItem;
            },
            
            handleCreateAssignment: function(e) {
                e.preventDefault();
                const title = document.getElementById('assignmentTitle').value;
                const description = document.getElementById('assignmentDescription').value;
                const dueDate = document.getElementById('assignmentDueDate').value;
                
                const newAssignment = {
                    id: db.generateId(),
                    title,
                    description,
                    dueDate,
                    teacherId: this.currentUser.id,
                    createdAt: new Date().toISOString()
                };
                
                db.addAssignment(newAssignment);
                this.loadAssignments();
                document.getElementById('createAssignmentForm').reset();
                
                // 添加提醒
                this.addReminder(`已创建新作业: ${title}`, new Date(dueDate));
            },
            
            prepareSubmissionForm: function(assignmentId) {
                document.getElementById('submitAssignmentId').value = assignmentId;
                document.getElementById('assignmentSubmission').value = '';
                document.getElementById('assignmentFile').value = '';
                
                // 滚动到提交表单
                document.getElementById('studentAssignmentSubmit').scrollIntoView({ behavior: 'smooth' });
            },
            
            handleSubmitAssignment: function(e) {
                e.preventDefault();
                const assignmentId = document.getElementById('submitAssignmentId').value;
                const content = document.getElementById('assignmentSubmission').value;
                const fileInput = document.getElementById('assignmentFile');
                
                // 简单的文件验证
                if (fileInput.files.length > 0) {
                    const file = fileInput.files[0];
                    if (file.size > 5 * 1024 * 1024) { // 5MB限制
                        document.getElementById('fileValidationError').textContent = '文件大小不能超过5MB';
                        return;
                    }
                    
                    const allowedTypes = ['application/pdf', 'application/msword', 'application/vnd.openxmlformats-officedocument.wordprocessingml.document'];
                    if (!allowedTypes.includes(file.type)) {
                        document.getElementById('fileValidationError').textContent = '只允许上传PDF或Word文档';
                        return;
                    }
                }
                
                const newSubmission = {
                    id: db.generateId(),
                    assignmentId,
                    studentId: this.currentUser.id,
                    content,
                    fileName: fileInput.files.length > 0 ? fileInput.files[0].name : null,
                    submittedAt: new Date().toISOString(),
                    graded: false
                };
                
                db.addSubmission(newSubmission);
                this.loadAssignments();
                document.getElementById('submitAssignmentForm').reset();
                
                // 添加提醒
                const assignment = db.getAssignmentById(assignmentId);
                this.addReminder(`已提交作业: ${assignment.title}`, new Date());
            },
            
            validateFileUpload: function(e) {
                const fileInput = e.target;
                document.getElementById('fileValidationError').textContent = '';
                
                if (fileInput.files.length > 0) {
                    const file = fileInput.files[0];
                    
                    // 验证文件大小
                    if (file.size > 5 * 1024 * 1024) { // 5MB限制
                        document.getElementById('fileValidationError').textContent = '文件大小不能超过5MB';
                        fileInput.value = '';
                        return;
                    }
                    
                    // 验证文件类型
                    const allowedTypes = ['application/pdf', 'application/msword', 'application/vnd.openxmlformats-officedocument.wordprocessingml.document'];
                    if (!allowedTypes.includes(file.type)) {
                        document.getElementById('fileValidationError').textContent = '只允许上传PDF或Word文档';
                        fileInput.value = '';
                        return;
                    }
                }
            },
            
            viewSubmissions: function(assignmentId) {
                const submissions = db.getSubmissionsByAssignment(assignmentId);
                const assignment = db.getAssignmentById(assignmentId);
                
                // 显示作业标题
                document.getElementById('gradingAssignmentTitle').textContent = assignment.title;
                
                // 默认显示第一个提交
                if (submissions.length > 0) {
                    this.displaySubmissionForGrading(submissions[0]);
                }
                
                // 滚动到批改区域
                document.getElementById('teacherGradingSection').scrollIntoView({ behavior: 'smooth' });
            },
            
            displaySubmissionForGrading: function(submission) {
                document.getElementById('gradingSubmissionId').value = submission.id;
                
                const student = db.getUsers().find(u => u.id === submission.studentId);
                document.getElementById('gradingStudentName').textContent = `学生: ${student.name}`;
                
                document.getElementById('gradingSubmissionContent').textContent = submission.content;
                
                const fileDiv = document.getElementById('gradingSubmissionFile');
                fileDiv.innerHTML = '';
                if (submission.fileName) {
                    const fileLink = document.createElement('a');
                    fileLink.href = '#'; // 实际应用中这里应该是文件URL
                    fileLink.textContent = `下载附件: ${submission.fileName}`;
                    fileLink.addEventListener('click', (e) => {
                        e.preventDefault();
                        alert('在实际应用中，这里会下载文件');
                    });
                    fileDiv.appendChild(fileLink);
                }
                
                // 如果已评分，显示分数和反馈
                if (submission.graded) {
                    document.getElementById('grade').value = submission.grade;
                    document.getElementById('feedback').value = submission.feedback;
                } else {
                    document.getElementById('grade').value = '';
                    document.getElementById('feedback').value = '';
                }
            },
            
            handleGradeSubmission: function(e) {
                e.preventDefault();
                const submissionId = document.getElementById('gradingSubmissionId').value;
                const grade = document.getElementById('grade').value;
                const feedback = document.getElementById('feedback').value;
                
                const submission = db.getSubmissionById(submissionId);
                if (!submission) return;
                
                const updatedSubmission = {
                    ...submission,
                    grade,
                    feedback,
                    graded: true,
                    gradedAt: new Date().toISOString()
                };
                
                db.updateSubmission(updatedSubmission);
                this.loadAssignments();
                
                // 添加提醒
                const assignment = db.getAssignmentById(submission.assignmentId);
                this.addReminder(`已批改作业: ${assignment.title}`, new Date());
            },
            
            // 提醒功能
            loadReminders: function() {
                const remindersContainer = document.getElementById('assignmentReminders');
                remindersContainer.innerHTML = '';
                
                // 示例提醒 - 在实际应用中，这些应从数据库加载
                if (this.currentUser.role === 'student') {
                    this.addReminder('作业1即将到期 - 截止日期: 明天', new Date());
                    this.addReminder('你提交的作业2已被评分', new Date());
                } else {
                    this.addReminder('3份作业等待批改', new Date());
                    this.addReminder('作业3截止日期: 2天后', new Date());
                }
            },
            
            addReminder: function(text, date) {
                const remindersContainer = document.getElementById('assignmentReminders');
                const reminder = document.createElement('div');
                reminder.className = 'reminder';
                reminder.innerHTML = `
                    <strong>${new Date(date).toLocaleDateString()}</strong>
                    <p>${text}</p>
                `;
                remindersContainer.appendChild(reminder);
            },
            
            // 消息系统
            loadMessageContacts: function() {
                const users = db.getUsers();
                const messageContacts = document.getElementById('messageContacts');
                messageContacts.innerHTML = '';
                
                // 过滤掉当前用户
                const contacts = users.filter(user => user.id !== this.currentUser.id);
                
                contacts.forEach(user => {
                    const contactItem = document.createElement('li');
                    contactItem.textContent = `${user.name} (${user.role === 'teacher' ? '教师' : '学生'})`;
                    contactItem.style.cursor = 'pointer';
                    contactItem.addEventListener('click', () => this.loadConversation(user.id));
                    messageContacts.appendChild(contactItem);
                });
            },
            
            loadConversation: function(otherUserId) {
                const otherUser = db.getUsers().find(u => u.id === otherUserId);
                document.getElementById('currentConversation').textContent = `与 ${otherUser.name} 的对话`;
                
                const messages = db.getMessagesBetweenUsers(this.currentUser.id, otherUserId);
                const messageHistory = document.getElementById('messageHistory');
                messageHistory.innerHTML = '';
                
                messages.forEach(message => {
                    const messageDiv = document.createElement('div');
                    messageDiv.className = `message ${message.senderId === this.currentUser.id ? 'sent' : 'received'}`;
                    messageDiv.innerHTML = `
                        <p>${message.text}</p>
                        <small>${new Date(message.timestamp).toLocaleString()}</small>
                    `;
                    messageHistory.appendChild(messageDiv);
                });
                
                // 显示发送消息表单并设置接收者ID
                document.getElementById('sendMessageForm').style.display = 'block';
                document.getElementById('receiverId').value = otherUserId;
                
                // 滚动到底部
                messageHistory.scrollTop = messageHistory.scrollHeight;
            },
            
            handleSendMessage: function(e) {
                e.preventDefault();
                const receiverId = document.getElementById('receiverId').value;
                const text = document.getElementById('messageText').value;
                
                const newMessage = {
                    id: db.generateId(),
                    senderId: this.currentUser.id,
                    receiverId,
                    text,
                    timestamp: new Date().toISOString()
                };
                
                db.addMessage(newMessage);
                this.loadConversation(receiverId);
                document.getElementById('messageText').value = '';
            },
            
            // 个人资料
            loadProfileData: function() {
                document.getElementById('profileName').value = this.currentUser.name;
                document.getElementById('profileEmail').value = this.currentUser.email;
                document.getElementById('profileRole').value = this.currentUser.role === 'teacher' ? '教师' : '学生';
            },
            
            handleUpdateProfile: function(e) {
                e.preventDefault();
                const name = document.getElementById('profileName').value;
                const currentPassword = document.getElementById('currentPassword').value;
                const newPassword = document.getElementById('newPassword').value;
                const confirmNewPassword = document.getElementById('confirmNewPassword').value;
                
                // 验证密码更改
                if (newPassword || confirmNewPassword) {
                    if (newPassword !== confirmNewPassword) {
                        document.getElementById('profileError').textContent = '新密码不匹配';
                        return;
                    }
                    
                    if (currentPassword !== this.currentUser.password) {
                        document.getElementById('profileError').textContent = '当前密码不正确';
                        return;
                    }
                }
                
                // 更新用户
                const updatedUser = {
                    ...this.currentUser,
                    name,
                    password: newPassword || this.currentUser.password
                };
                
                db.updateUser(updatedUser);
                this.currentUser = updatedUser;
                localStorage.setItem('currentUser', JSON.stringify(updatedUser));
                
                document.getElementById('profileError').textContent = '';
                document.getElementById('profileForm').reset();
                this.loadProfileData();
                
                alert('个人资料已更新');
            }
        };

        // 初始化应用
        document.addEventListener('DOMContentLoaded', function() {
            app.init();
        });
    </script>
</body>
</html>
