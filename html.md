<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Student Portal - Admin Dashboard (Frontend Demo)</title>
  <style>
    /* Simple, clean styles suitable for online compilers */
    :root{--bg:#f4f7fb;--card:#fff;--accent:#2b6ef6;--muted:#7a8396}
    *{box-sizing:border-box;font-family:Inter, system-ui, Arial;}
    body{margin:0;background:var(--bg);color:#10203a}
    header{background:linear-gradient(90deg,var(--accent),#4fb3ff);color:#fff;padding:14px 20px;display:flex;align-items:center;justify-content:space-between}
    header h1{margin:0;font-size:18px}
    .wrap{max-width:1200px;margin:18px auto;padding:12px}
    .grid{display:grid;grid-template-columns:260px 1fr;gap:16px}
    nav{background:var(--card);padding:12px;border-radius:10px;box-shadow:0 6px 20px rgba(20,30,60,.06)}
    nav button{display:block;width:100%;text-align:left;padding:10px;border:0;background:transparent;border-radius:6px;margin-bottom:6px;cursor:pointer}
    nav button.active{background:linear-gradient(90deg,#eef6ff,#f7fbff);color:var(--accent);font-weight:600}
    main{min-height:600px}
    .card{background:var(--card);padding:14px;border-radius:10px;box-shadow:0 6px 20px rgba(20,30,60,.05)}
    .row{display:flex;gap:12px;align-items:center}
    .small{font-size:13px;color:var(--muted)}
    table{width:100%;border-collapse:collapse}
    th,td{padding:8px;border-bottom:1px solid #eef2f9;text-align:left}
    input,select,textarea{padding:8px;border:1px solid #e6eefc;border-radius:6px;width:100%}
    .btn{padding:8px 10px;border-radius:8px;border:0;background:var(--accent);color:#fff;cursor:pointer}
    .btn.secondary{background:#f3f6fb;color:#10203a}
    .stats{display:flex;gap:12px}
    .stat{flex:1;padding:12px;border-radius:8px;background:linear-gradient(180deg,#fff,#fbfdff);}
    .muted{color:var(--muted)}
    .kpi{font-size:22px;font-weight:700}
    .actions{display:flex;gap:8px}
    .danger{background:#ff6b6b;color:#fff}
    .chip{display:inline-block;padding:6px 8px;border-radius:999px;background:#f1f5ff;color:var(--accent);font-weight:600}
    .topbar{display:flex;gap:8px;align-items:center}
    .file{font-size:13px;color:var(--muted)}
    /* responsive */
    @media(max-width:880px){.grid{grid-template-columns:1fr;}.stat{font-size:14px}}
  </style>
</head>
<body>
  <header>
    <h1>Student Portal — Admin</h1>
    <div class="topbar">
      <div class="small">Logged as <strong>Admin</strong></div>
      <button id="logout" class="btn secondary">Logout</button>
    </div>
  </header>

  <div class="wrap">
    <div class="grid">
      <nav class="card" id="menu">
        <button data-view="dashboard" class="active">Dashboard</button>
        <button data-view="students">Student Management</button>
        <button data-view="courses">Courses & Subjects</button>
        <button data-view="attendance">Attendance</button>
        <button data-view="marks">Marks & Results</button>
        <button data-view="faculty">Faculty Management</button>
        <button data-view="timetable">Timetable</button>
        <button data-view="fees">Fee Management</button>
        <button data-view="exams">Exam Cell</button>
        <button data-view="documents">Documents</button>
        <button data-view="requests">Student Requests</button>
        <button data-view="placements">Placement Cell</button>
        <button data-view="library">Library</button>
        <button data-view="hostel">Hostel & Transport</button>
        <button data-view="security">Security & Logs</button>
      </nav>

      <main id="content">
        <!-- Views injected by JS. Minimal default dashboard markup here -->
        <section id="dashboard" class="card view">
          <div class="row" style="justify-content:space-between">
            <div>
              <h2 style="margin:0">Admin Dashboard</h2>
              <div class="small">Overview of academic operations</div>
            </div>
            <div class="row">
              <input id="quickSearch" placeholder="Quick search students..." />
              <button class="btn" id="exportAll">Export All Data</button>
            </div>
          </div>

          <div style="height:18px"></div>

          <div class="stats">
            <div class="stat"><div class="small">Total Students</div><div id="kTotalStudents" class="kpi">0</div></div>
            <div class="stat"><div class="small">Total Faculty</div><div id="kTotalFaculty" class="kpi">0</div></div>
            <div class="stat"><div class="small">Pending Fees</div><div id="kPendingFees" class="kpi">0</div></div>
            <div class="stat"><div class="small">Avg Attendance</div><div id="kAvgAtt" class="kpi">0%</div></div>
          </div>

          <div style="margin-top:14px" id="recentNotes" class="card">
            <h3 style="margin-top:0">Recent Notifications</h3>
            <div id="notesList" class="small muted">No notifications yet</div>
          </div>
        </section>

        <!-- Students view -->
        <section id="students" class="card view" style="display:none">
          <div style="display:flex;justify-content:space-between;align-items:center">
            <h2 style="margin:0">Student Management</h2>
            <div class="row">
              <button id="addStudentBtn" class="btn">Add Student</button>
              <button id="importStudents" class="btn secondary">Import CSV</button>
            </div>
          </div>
          <div style="height:10px"></div>
          <div class="card">
            <input id="filterStudent" placeholder="Filter by name / id" />
            <div style="height:8px"></div>
            <table id="studentTable">
              <thead><tr><th>ID</th><th>Name</th><th>Course</th><th>Phone</th><th>Attendance%</th><th>Fees</th><th>Actions</th></tr></thead>
              <tbody></tbody>
            </table>
          </div>
        </section>

        <!-- Courses view -->
        <section id="courses" class="card view" style="display:none">
          <h2 style="margin:0">Courses & Subjects</h2>
          <div style="height:10px"></div>
          <div style="display:flex;gap:12px">
            <div style="flex:1">
              <div class="card">
                <h3>Add Course</h3>
                <input id="courseName" placeholder="Course name (e.g., CSE)" />
                <div style="height:8px"></div>
                <button id="saveCourse" class="btn">Save Course</button>
              </div>
            </div>
            <div style="flex:2">
              <div class="card">
                <h3>Courses</h3>
                <table id="coursesTable"><thead><tr><th>Course</th><th>Subjects</th><th>Actions</th></tr></thead><tbody></tbody></table>
              </div>
            </div>
          </div>
        </section>

        <!-- Attendance view -->
        <section id="attendance" class="card view" style="display:none">
          <h2>Attendance Management</h2>
          <div style="display:flex;gap:12px;align-items:center">
            <input id="attDate" type="date" />
            <select id="attCourse"></select>
            <button id="takeAttendance" class="btn">Load Students</button>
          </div>
          <div style="height:8px"></div>
          <div class="card" id="attendanceList"></div>
        </section>

        <!-- Marks view -->
        <section id="marks" class="card view" style="display:none">
          <h2>Marks & Results</h2>
          <div style="display:flex;gap:12px;align-items:center">
            <select id="marksCourse"></select>
            <select id="marksSubject"></select>
            <button id="loadMarks" class="btn">Load Students</button>
          </div>
          <div id="marksList" class="card" style="margin-top:8px"></div>
        </section>

        <!-- Faculty view -->
        <section id="faculty" class="card view" style="display:none">
          <h2>Faculty Management</h2>
          <div style="display:flex;gap:12px">
            <div style="flex:1" class="card">
              <h3>Add Faculty</h3>
              <input id="facName" placeholder="Name" />
              <input id="facEmail" placeholder="Email" />
              <input id="facPhone" placeholder="Phone" />
              <button id="saveFaculty" class="btn">Save</button>
            </div>
            <div style="flex:2" class="card">
              <h3>Faculty List</h3>
              <table id="facultyTable"><thead><tr><th>Name</th><th>Email</th><th>Phone</th><th>Assign</th></tr></thead><tbody></tbody></table>
            </div>
          </div>
        </section>

        <!-- Timetable view -->
        <section id="timetable" class="card view" style="display:none">
          <h2>Timetable Management</h2>
          <div class="card">
            <textarea id="ttText" placeholder="Enter weekly timetable (free text or JSON)"></textarea>
            <div style="height:8px"></div>
            <button id="saveTT" class="btn">Save Timetable</button>
            <button id="downloadTT" class="btn secondary">Download Timetable</button>
          </div>
        </section>

        <!-- Fees view -->
        <section id="fees" class="card view" style="display:none">
          <h2>Fee Management</h2>
          <div class="card">
            <table id="feesTable"><thead><tr><th>ID</th><th>Name</th><th>Amount</th><th>Status</th><th>Actions</th></tr></thead><tbody></tbody></table>
          </div>
        </section>

        <!-- Exams -->
        <section id="exams" class="card view" style="display:none">
          <h2>Exam Cell</h2>
          <div style="display:flex;gap:12px">
            <input id="examName" placeholder="Exam name (e.g., Mid Sem)" />
            <input id="examDate" type="date" />
            <input id="examHall" placeholder="Hall" />
            <button id="createExam" class="btn">Create Exam</button>
          </div>
          <div style="height:12px"></div>
          <div class="card"><table id="examsTable"><thead><tr><th>Exam</th><th>Date</th><th>Hall</th><th>Actions</th></tr></thead><tbody></tbody></table></div>
        </section>

        <!-- Documents -->
        <section id="documents" class="card view" style="display:none">
          <h2>Document Uploads</h2>
          <div class="card">
            <input id="docTitle" placeholder="Title" />
            <input id="docFile" type="file" />
            <div style="height:8px"></div>
            <button id="uploadDoc" class="btn">Upload</button>
            <div style="height:8px"></div>
            <table id="docsTable"><thead><tr><th>Title</th><th>File</th><th>Actions</th></tr></thead><tbody></tbody></table>
          </div>
        </section>

        <!-- Requests -->
        <section id="requests" class="card view" style="display:none">
          <h2>Student Requests</h2>
          <div class="card">
            <h4>Raise Request (simulate as student)</h4>
            <select id="reqStudent"></select>
            <select id="reqType"><option>Bonafide certificate</option><option>Leave request</option><option>ID card reissue</option><option>Grievance</option></select>
            <textarea id="reqMsg" placeholder="Message"></textarea>
            <button id="raiseReq" class="btn">Raise</button>
          </div>
          <div style="height:8px"></div>
          <div class="card">
            <h4>Pending Requests</h4>
            <table id="reqTable"><thead><tr><th>Student</th><th>Type</th><th>Message</th><th>Status</th><th>Actions</th></tr></thead><tbody></tbody></table>
          </div>
        </section>

        <!-- Placements -->
        <section id="placements" class="card view" style="display:none">
          <h2>Placement Cell</h2>
          <div class="card">
            <input id="compName" placeholder="Company Name" />
            <input id="jobRole" placeholder="Role/Internship" />
            <input id="eligibility" placeholder="Eligibility (e.g., CGPA &gt;= 6)" />
            <button id="postJob" class="btn">Post</button>
            <div style="height:8px"></div>
            <table id="jobsTable"><thead><tr><th>Company</th><th>Role</th><th>Eligibility</th><th>Apply</th></tr></thead><tbody></tbody></table>
          </div>
        </section>

        <!-- Library -->
        <section id="library" class="card view" style="display:none">
          <h2>Library</h2>
          <div class="card">
            <input id="bookTitle" placeholder="Book title" />
            <input id="bookAuthor" placeholder="Author" />
            <button id="addBook" class="btn">Add Book</button>
            <div style="height:8px"></div>
            <table id="booksTable"><thead><tr><th>Title</th><th>Author</th><th>Avail</th></tr></thead><tbody></tbody></table>
          </div>
        </section>

        <!-- Hostel -->
        <section id="hostel" class="card view" style="display:none">
          <h2>Hostel & Transport</h2>
          <div class="card">
            <h4>Room Allocation (simple)</h4>
            <input id="roomStudent" placeholder="Student ID" />
            <input id="roomNo" placeholder="Room no" />
            <button id="allocRoom" class="btn">Allocate</button>
            <div style="height:8px"></div>
            <table id="roomsTable"><thead><tr><th>Student</th><th>Room</th></tr></thead><tbody></tbody></table>
          </div>
        </section>

        <!-- Security -->
        <section id="security" class="card view" style="display:none">
          <h2>Security & Logs</h2>
          <div class="card">
            <h4>Users & Roles</h4>
            <table id="usersTable"><thead><tr><th>Username</th><th>Role</th><th>Actions</th></tr></thead><tbody></tbody></table>
            <div style="height:8px"></div>
            <h4>Activity Logs (last 200 actions)</h4>
            <div id="logsArea" style="max-height:220px;overflow:auto;background:#fff;padding:8px;border-radius:6px"></div>
          </div>
        </section>

      </main>
    </div>
  </div>

  <template id="studentRowTpl">
    <tr>
      <td class="s-id"></td>
      <td class="s-name"></td>
      <td class="s-course"></td>
      <td class="s-phone"></td>
      <td class="s-att"></td>
      <td class="s-fees"></td>
      <td class="s-actions"></td>
    </tr>
  </template>

  <script>
    // Basic frontend-only Student Portal demo using localStorage as "backend".
    // This is a lightweight single-file app to run on online HTML playgrounds.

    // Utilities
    const $ = sel => document.querySelector(sel);
    const $$ = sel => Array.from(document.querySelectorAll(sel));
    function sanitize(s){return String(s||'').replace(/</g,'&lt;').replace(/>/g,'&gt;');}
    function uid(prefix='id'){return prefix+Math.random().toString(36).slice(2,9)}
    function log(action){
      const logs = JSON.parse(localStorage.getItem('logs')||'[]');
      logs.unshift({id:uid('log'),time:new Date().toISOString(),action});
      localStorage.setItem('logs',JSON.stringify(logs.slice(0,200)));
      renderLogs();
    }

    // Initial data setup
    function seed(){
      if(localStorage.getItem('init')) return;
      const students=[
        {id:'S001',name:'Sravani',course:'CSE',phone:'9999999999',feesDue:0},
        {id:'S002',name:'Anirudh',course:'CSE',phone:'8888888888',feesDue:1000},
        {id:'S003',name:'Bhavya',course:'ECE',phone:'7777777777',feesDue:500}
      ];
      const faculty=[{id:'F1',name:'Prof. Rao',email:'rao@uni',phone:'7000000000'}];
      const courses=[{name:'CSE',subjects:['DSA','OS','DBMS']},{name:'ECE',subjects:['Signals','Networks']}];
      localStorage.setItem('students',JSON.stringify(students));
      localStorage.setItem('faculty',JSON.stringify(faculty));
      localStorage.setItem('courses',JSON.stringify(courses));
      localStorage.setItem('notifications',JSON.stringify([]));
      localStorage.setItem('users',JSON.stringify([{username:'admin',role:'admin'},{username:'faculty',role:'faculty'},{username:'student',role:'student'}]));
      localStorage.setItem('init', '1');
      log('Initial data seeded');
    }

    // Data helpers
    function getStudents(){return JSON.parse(localStorage.getItem('students')||'[]')}
    function setStudents(arr){localStorage.setItem('students',JSON.stringify(arr));}
    function getFaculty(){return JSON.parse(localStorage.getItem('faculty')||'[]')}
    function setFaculty(arr){localStorage.setItem('faculty',JSON.stringify(arr));}
    function getCourses(){return JSON.parse(localStorage.getItem('courses')||'[]')}
    function setCourses(arr){localStorage.setItem('courses',JSON.stringify(arr));}
    function getNotifications(){return JSON.parse(localStorage.getItem('notifications')||'[]')}
    function setNotifications(n){localStorage.setItem('notifications',JSON.stringify(n));}

    // Render helpers
    function renderDashboard(){
      const st = getStudents();
      const fa = getFaculty();
      const nots = getNotifications();
      $('#kTotalStudents').textContent = st.length;
      $('#kTotalFaculty').textContent = fa.length;
      const pending = st.reduce((s,x)=>s+(x.feesDue||0),0);
      $('#kPendingFees').textContent = pending;
      // attendance average (naive)
      const att = JSON.parse(localStorage.getItem('attendance')||'{}');
      let avg=0;let count=0;for(const id in att){const arr=att[id]; if(arr.length){avg+=arr.reduce((a,b)=>a+(b.present?1:0),0)/arr.length;count++}}
      $('#kAvgAtt').textContent = count?Math.round((avg/count)*100)+'%':'0%';
      $('#notesList').innerHTML = nots.length?'<ul>'+nots.slice(0,6).map(n=>'<li>'+sanitize(n.title)+' — <span class="small muted">'+n.date+'</span></li>').join('')+'</ul>':'No notifications yet';
    }

    function renderStudents(filter=''){
      const tbody = $('#studentTable tbody'); tbody.innerHTML='';
      const students = getStudents().filter(s=> (s.name+s.id+ (s.course||'')).toLowerCase().includes(filter.toLowerCase()));
      students.forEach(s=>{
        const tr = document.importNode($('#studentRowTpl').content,true);
        tr.querySelector('.s-id').textContent = s.id;
        tr.querySelector('.s-name').textContent = s.name;
        tr.querySelector('.s-course').textContent = s.course||'';
        tr.querySelector('.s-phone').textContent = s.phone||'';
        const attPercent = computeAttendancePercent(s.id);
        tr.querySelector('.s-att').textContent = attPercent+'%';
        tr.querySelector('.s-fees').textContent = s.feesDue?('₹'+s.feesDue):'Paid';
        const actions = tr.querySelector('.s-actions');
        const edit = document.createElement('button'); edit.textContent='Edit'; edit.className='btn secondary'; edit.onclick=()=>openEditStudent(s.id);
        const del = document.createElement('button'); del.textContent='Delete'; del.className='btn danger'; del.onclick=()=>deleteStudent(s.id);
        const view = document.createElement('button'); view.textContent='Profile'; view.className='btn'; view.onclick=()=>viewProfile(s.id);
        actions.appendChild(view); actions.appendChild(edit); actions.appendChild(del);
        tbody.appendChild(tr);
      });
      // populate selects for requests/rooms
      const selS = $('#reqStudent'); selS.innerHTML=''; getStudents().forEach(s=>{const o=document.createElement('option');o.value=s.id;o.textContent=s.id+' — '+s.name;selS.appendChild(o)});
      renderDashboard();
    }

    function computeAttendancePercent(sid){
      const att = JSON.parse(localStorage.getItem('attendance')||'{}');
      const arr = att[sid]||[]; if(!arr.length) return 0; const present=arr.filter(x=>x.present).length; return Math.round((present/arr.length)*100);
    }

    function openEditStudent(id){
      const students=getStudents(); const s=students.find(x=>x.id===id); if(!s) return alert('Not found');
      const name = prompt('Name', s.name); if(name===null) return; s.name = sanitize(name);
      const phone = prompt('Phone', s.phone||''); if(phone!==null) s.phone = sanitize(phone);
      setStudents(students); log('Edited student '+id); renderStudents($('#filterStudent').value);
    }
    function deleteStudent(id){ if(!confirm('Delete '+id+'?'))return; let arr=getStudents(); arr=arr.filter(x=>x.id!==id); setStudents(arr); log('Deleted student '+id); renderStudents($('#filterStudent').value); }
    function viewProfile(id){ const s=getStudents().find(x=>x.id===id); alert('Profile:\nID: '+s.id+'\nName: '+s.name+'\nCourse: '+(s.course||'')+'\nPhone: '+(s.phone||'')+'\nFees Due: '+(s.feesDue||0));}

    // Courses
    function renderCourses(){ const tbody = $('#coursesTable tbody'); tbody.innerHTML=''; getCourses().forEach(c=>{ const tr=document.createElement('tr'); tr.innerHTML='<td>'+sanitize(c.name)+'</td><td>'+sanitize((c.subjects||[]).join(', '))+'</td><td><button class="btn" onclick="editCourse(&quot;'+c.name+'&quot;)">Edit</button></td>'; tbody.appendChild(tr); });
      const sel = $('#attCourse'); sel.innerHTML=''; getCourses().forEach(c=>{const o=document.createElement('option'); o.value=c.name; o.textContent=c.name; sel.appendChild(o)});
      const msel = $('#marksCourse'); msel.innerHTML=''; getCourses().forEach(c=>{const o=document.createElement('option'); o.value=c.name; o.textContent=c.name; msel.appendChild(o)});
    }
    window.editCourse = function(name){ const courses=getCourses(); const c=courses.find(x=>x.name===name); const sub=prompt('Subjects (comma separated)', (c.subjects||[]).join(', ')); if(sub===null) return; c.subjects = sub.split(',').map(s=>s.trim()); setCourses(courses); log('Edited course '+name); renderCourses(); }

    // Faculty
    function renderFaculty(){ const tbody=$('#facultyTable tbody'); tbody.innerHTML=''; getFaculty().forEach(f=>{const tr=document.createElement('tr'); tr.innerHTML='<td>'+sanitize(f.name)+'</td><td>'+sanitize(f.email)+'</td><td>'+sanitize(f.phone)+'</td><td><button class="btn" onclick="assignFaculty(\''+f.id+'\')">Assign</button></td>'; tbody.appendChild(tr);} );
      const ut = $('#usersTable tbody'); ut.innerHTML=''; JSON.parse(localStorage.getItem('users')||'[]').forEach(u=>{const tr=document.createElement('tr'); tr.innerHTML='<td>'+sanitize(u.username)+'</td><td>'+sanitize(u.role)+'</td><td><button class="btn" onclick="removeUser(\''+u.username+'\')">Remove</button></td>'; ut.appendChild(tr);});
    }
    window.assignFaculty = function(id){ const subj = prompt('Assign subject (e.g., DSA)'); if(!subj) return; log('Assigned faculty '+id+' to '+subj); alert('Assigned (simulated)'); }

    // Attendance handling
    function renderAttendanceList(date, course){ const container = $('#attendanceList'); container.innerHTML=''; const students = getStudents().filter(s=>!course||s.course===course);
      if(students.length===0) return container.textContent='No students found';
      const tbl = document.createElement('table'); tbl.innerHTML='<thead><tr><th>ID</th><th>Name</th><th>Present</th></tr></thead>';
      const tb = document.createElement('tbody'); students.forEach(s=>{ const tr=document.createElement('tr'); tr.innerHTML='<td>'+sanitize(s.id)+'</td><td>'+sanitize(s.name)+'</td><td></td>'; const chk = document.createElement('input'); chk.type='checkbox'; chk.checked = getAttendanceFor(s.id,date).present||false; chk.onchange=()=>saveAttendanceFor(s.id,date,chk.checked); tr.children[2].appendChild(chk); tb.appendChild(tr); }); tbl.appendChild(tb); container.appendChild(tbl);
    }
    function getAttendanceFor(sid,date){ const att=JSON.parse(localStorage.getItem('attendance')||'{}'); const arr=att[sid]||[]; const rec=arr.find(x=>x.date===date); return rec||{date,present:false}; }
    function saveAttendanceFor(sid,date,present){ const att=JSON.parse(localStorage.getItem('attendance')||'{}'); if(!att[sid]) att[sid]=[]; const idx = att[sid].findIndex(x=>x.date===date); if(idx>-1) att[sid][idx].present=present; else att[sid].push({date,present}); localStorage.setItem('attendance',JSON.stringify(att)); log('Attendance updated for '+sid+' on '+date); renderStudents($('#filterStudent').value); }

    // Marks
    function renderMarks(course,subject){ const container = $('#marksList'); container.innerHTML=''; const studs=getStudents().filter(s=>s.course===course);
      if(!subject) return container.textContent='Select subject';
      const tbl=document.createElement('table'); tbl.innerHTML='<thead><tr><th>ID</th><th>Name</th><th>Marks</th></tr></thead>';
      const tb=document.createElement('tbody'); const marksAll=JSON.parse(localStorage.getItem('marks')||'{}'); studs.forEach(s=>{ const tr=document.createElement('tr'); const val=(marksAll[s.id] && marksAll[s.id][subject])||''; tr.innerHTML='<td>'+sanitize(s.id)+'</td><td>'+sanitize(s.name)+'</td><td></td>'; const inp=document.createElement('input'); inp.value=val; inp.onchange=()=>{ if(!marksAll[s.id]) marksAll[s.id]={}; marksAll[s.id][subject]=inp.value; localStorage.setItem('marks',JSON.stringify(marksAll)); log('Marks updated for '+s.id+' subject '+subject); }; tr.children[2].appendChild(inp); tb.appendChild(tr); }); tbl.appendChild(tb); container.appendChild(tbl);
    }

    // Exams
    function renderExams(){ const t=$('#examsTable tbody'); t.innerHTML=''; const ex=JSON.parse(localStorage.getItem('exams')||'[]'); ex.forEach(e=>{ const tr=document.createElement('tr'); tr.innerHTML='<td>'+sanitize(e.name)+'</td><td>'+sanitize(e.date)+'</td><td>'+sanitize(e.hall)+'</td><td><button class="btn" onclick="generateHallTicket(\''+e.name+'\')">Hall Ticket</button></td>'; t.appendChild(tr); }); }
    window.generateHallTicket = function(exam){ const studs=getStudents(); const html = 'Hall Ticket - '+exam+'\n\n'+studs.map(s=>s.id+' - '+s.name).join('\n'); // simple
      const blob = new Blob([html],{type:'text/plain'}); const url = URL.createObjectURL(blob); const a=document.createElement('a'); a.href=url; a.download='halltickets_'+exam+'.txt'; a.click(); log('Generated hall tickets for '+exam);
    }

    // Documents
    function renderDocs(){ const t=$('#docsTable tbody'); t.innerHTML=''; const docs=JSON.parse(localStorage.getItem('documents')||'[]'); docs.forEach(d=>{ const tr=document.createElement('tr'); tr.innerHTML='<td>'+sanitize(d.title)+'</td><td>'+sanitize(d.name)+'</td><td><button class="btn" onclick="downloadDoc(\''+d.id+'\')">Download</button></td>'; t.appendChild(tr); }); }
    window.downloadDoc = function(id){ const docs=JSON.parse(localStorage.getItem('documents')||'[]'); const d=docs.find(x=>x.id===id); if(!d) return; const blob = b64toBlob(d.data,d.type); const url=URL.createObjectURL(blob); const a=document.createElement('a'); a.href=url; a.download=d.name; a.click(); log('Downloaded document '+d.name); }
    function b64toBlob(b64Data, contentType='', sliceSize=512){ const byteCharacters = atob(b64Data); const byteArrays = []; for (let offset = 0; offset < byteCharacters.length; offset += sliceSize) { const slice = byteCharacters.slice(offset, offset + sliceSize); const byteNumbers = new Array(slice.length); for (let i = 0; i < slice.length; i++) byteNumbers[i] = slice.charCodeAt(i); const byteArray = new Uint8Array(byteNumbers); byteArrays.push(byteArray); } return new Blob(byteArrays, {type: contentType}); }

    // Requests
    function renderRequests(){ const t=$('#reqTable tbody'); t.innerHTML=''; const reqs=JSON.parse(localStorage.getItem('requests')||'[]'); reqs.forEach(r=>{ const tr=document.createElement('tr'); tr.innerHTML='<td>'+sanitize(r.studentId)+'</td><td>'+sanitize(r.type)+'</td><td>'+sanitize(r.msg)+'</td><td>'+sanitize(r.status||'Pending')+'</td><td><button class="btn" onclick="resolveReq(\''+r.id+'\')">Approve</button><button class="btn secondary" onclick="rejectReq(\''+r.id+'\')">Reject</button></td>'; t.appendChild(tr); }); }
    window.resolveReq = function(id){ const reqs=JSON.parse(localStorage.getItem('requests')||'[]'); const r=reqs.find(x=>x.id===id); if(r){r.status='Approved'; localStorage.setItem('requests',JSON.stringify(reqs)); log('Approved request '+id); renderRequests(); }}
    window.rejectReq = function(id){ const reqs=JSON.parse(localStorage.getItem('requests')||'[]'); const r=reqs.find(x=>x.id===id); if(r){r.status='Rejected'; localStorage.setItem('requests',JSON.stringify(reqs)); log('Rejected request '+id); renderRequests(); }}

    // Placements
    function renderJobs(){ const t=$('#jobsTable tbody'); t.innerHTML=''; const jobs=JSON.parse(localStorage.getItem('jobs')||'[]'); jobs.forEach(j=>{ const tr=document.createElement('tr'); tr.innerHTML='<td>'+sanitize(j.company)+'</td><td>'+sanitize(j.role)+'</td><td>'+sanitize(j.eligibility)+'</td><td><button class="btn" onclick="applyJob(\''+j.id+'\')">Apply</button></td>'; t.appendChild(tr); }); }
    window.applyJob=function(id){ alert('Applied (simulated)'); log('Applied for job '+id); }

    // Library
    function renderBooks(){ const t=$('#booksTable tbody'); t.innerHTML=''; const books=JSON.parse(localStorage.getItem('books')||'[]'); books.forEach(b=>{ const tr=document.createElement('tr'); tr.innerHTML='<td>'+sanitize(b.title)+'</td><td>'+sanitize(b.author)+'</td><td>'+(b.avail?'Yes':'No')+'</td>'; t.appendChild(tr); }); }

    // Rooms
    function renderRooms(){ const t=$('#roomsTable tbody'); t.innerHTML=''; const rooms=JSON.parse(localStorage.getItem('rooms')||'[]'); rooms.forEach(r=>{ const tr=document.createElement('tr'); tr.innerHTML='<td>'+sanitize(r.sid)+'</td><td>'+sanitize(r.room)+'</td>'; t.appendChild(tr); }); }

    // Logs
    function renderLogs(){ const area=$('#logsArea'); const logs=JSON.parse(localStorage.getItem('logs')||'[]'); area.innerHTML = logs.map(l=>'<div><strong>'+l.time+'</strong> - '+sanitize(l.action)+'</div>').join(''); }

    // Export
    function exportAll(){ const data={students:getStudents(),courses:getCourses(),faculty:getFaculty(),attendance:JSON.parse(localStorage.getItem('attendance')||'{}')}; const blob=new Blob([JSON.stringify(data,null,2)],{type:'application/json'}); const a=document.createElement('a'); a.href=URL.createObjectURL(blob); a.download='portal_export.json'; a.click(); log('Exported all data'); }

    // UI wiring
    function showView(name){ $$('.view').forEach(v=>v.style.display='none'); $('#'+name).style.display='block'; $$('#menu button').forEach(b=>b.classList.toggle('active', b.dataset.view===name)); }
    document.getElementById('menu').addEventListener('click',e=>{ if(e.target.tagName==='BUTTON'){ showView(e.target.dataset.view); }});

    // Buttons wiring and form handlers
    document.getElementById('addStudentBtn').onclick = ()=>{
      const id = prompt('Student ID (e.g., S010)'); if(!id) return; const name = prompt('Name'); if(!name) return; const course = prompt('Course (e.g., CSE)'); const phone = prompt('Phone'); const students=getStudents(); if(students.find(s=>s.id===id)) return alert('ID exists'); students.push({id:sanitize(id),name:sanitize(name),course:sanitize(course),phone:sanitize(phone),feesDue:0}); setStudents(students); log('Added student '+id); renderStudents(''); };

    document.getElementById('filterStudent').oninput = e=> renderStudents(e.target.value);
    $('#quickSearch').addEventListener('input',e=>{ showView('students'); renderStudents(e.target.value); });
    $('#saveCourse').onclick = ()=>{ const name = $('#courseName').value.trim(); if(!name) return alert('Enter name'); const courses=getCourses(); if(courses.find(c=>c.name===name)) return alert('Exists'); courses.push({name,subjects:[]}); setCourses(courses); log('Added course '+name); renderCourses(); };
    $('#takeAttendance').onclick = ()=>{ const date = $('#attDate').value; const course = $('#attCourse').value; if(!date) return alert('Choose date'); renderAttendanceList(date,course); };
    $('#loadMarks').onclick = ()=>{ const course = $('#marksCourse').value; const subject = $('#marksSubject').value; if(!course) return alert('Select course'); renderMarks(course,subject); };
    $('#marksCourse').onchange = e=>{ const course = e.target.value; const c = getCourses().find(x=>x.name===course); const sub = $('#marksSubject'); sub.innerHTML=''; (c?.subjects||[]).forEach(s=>{const o=document.createElement('option');o.value=s;o.textContent=s;sub.appendChild(o)}); };
    $('#saveFaculty').onclick = ()=>{ const name=$('#facName').value.trim(); const email=$('#facEmail').value.trim(); const phone=$('#facPhone').value.trim(); if(!name) return alert('Enter name'); const arr=getFaculty(); arr.push({id:uid('F'),name:sanitize(name),email:sanitize(email),phone:sanitize(phone)}); setFaculty(arr); log('Added faculty '+name); renderFaculty(); };
    $('#saveTT').onclick = ()=>{ localStorage.setItem('timetable',$('#ttText').value); log('Timetable updated'); alert('Saved (frontend)'); };
    $('#downloadTT').onclick = ()=>{ const t=localStorage.getItem('timetable')||''; const blob=new Blob([t],{type:'text/plain'}); const a=document.createElement('a'); a.href=URL.createObjectURL(blob); a.download='timetable.txt'; a.click(); log('Timetable downloaded'); };
    $('#createExam').onclick = ()=>{ const name=$('#examName').value.trim(); const date=$('#examDate').value; const hall=$('#examHall').value.trim(); if(!name) return; const ex=JSON.parse(localStorage.getItem('exams')||'[]'); ex.push({id:uid('E'),name,sanitize(date),hall}); localStorage.setItem('exams',JSON.stringify(ex)); log('Created exam '+name); renderExams(); };
    $('#uploadDoc').onclick = ()=>{ const f = $('#docFile').files[0]; const title = $('#docTitle').value.trim()||f?.name; if(!f) return alert('Choose file'); const reader = new FileReader(); reader.onload = function(e){ const data = e.target.result.split(',')[1]; const docs = JSON.parse(localStorage.getItem('documents')||'[]'); const rec={id:uid('D'),title:sanitize(title),name:sanitize(f.name),type:f.type,data}; docs.push(rec); localStorage.setItem('documents',JSON.stringify(docs)); log('Uploaded doc '+f.name); renderDocs(); }; reader.readAsDataURL(f); };
    $('#raiseReq').onclick = ()=>{ const sid = $('#reqStudent').value; const type = $('#reqType').value; const msg = $('#reqMsg').value.trim(); const reqs = JSON.parse(localStorage.getItem('requests')||'[]'); reqs.push({id:uid('R'),studentId:sid,type: sanitize(type),msg:sanitize(msg),status:'Pending'}); localStorage.setItem('requests',JSON.stringify(reqs)); log('Request raised by '+sid); renderRequests(); };
    $('#postJob').onclick = ()=>{ const comp=$('#compName').value.trim(); const role=$('#jobRole').value.trim(); const el=$('#eligibility').value.trim(); if(!comp) return alert('Enter company'); const jobs=JSON.parse(localStorage.getItem('jobs')||'[]'); jobs.push({id:uid('J'),company:sanitize(comp),role:sanitize(role),eligibility:sanitize(el)}); localStorage.setItem('jobs',JSON.stringify(jobs)); log('Posted job '+comp); renderJobs(); };
    $('#addBook').onclick = ()=>{ const t=$('#bookTitle').value.trim(); const a=$('#bookAuthor').value.trim(); if(!t) return alert('Enter title'); const books=JSON.parse(localStorage.getItem('books')||'[]'); books.push({id:uid('B'),title:sanitize(t),author:sanitize(a),avail:true}); localStorage.setItem('books',JSON.stringify(books)); log('Added book '+t); renderBooks(); };
    $('#allocRoom').onclick = ()=>{ const sid=$('#roomStudent').value.trim(); const room=$('#roomNo').value.trim(); if(!sid||!room) return alert('Enter both'); const rooms=JSON.parse(localStorage.getItem('rooms')||'[]'); rooms.push({id:uid('RM'),sid:sanitize(sid),room:sanitize(room)}); localStorage.setItem('rooms',JSON.stringify(rooms)); log('Allocated room '+room+' to '+sid); renderRooms(); };
    $('#exportAll').onclick = exportAll;
    $('#importStudents').onclick = ()=>{ const csv = prompt('Paste CSV rows: id,name,course,phone per line'); if(!csv) return; const lines=csv.trim().split('\n'); const arr=getStudents(); lines.forEach(l=>{ const [id,name,course,phone]=l.split(',').map(x=>x&&x.trim()); if(id) arr.push({id:sanitize(id),name:sanitize(name),course:sanitize(course),phone:sanitize(phone),feesDue:0}); }); setStudents(arr); log('Imported students'); renderStudents(''); };

    // Security small features
    function renderUsers(){ const ut = $('#usersTable tbody'); ut.innerHTML=''; const users=JSON.parse(localStorage.getItem('users')||'[]'); users.forEach(u=>{ const tr=document.createElement('tr'); tr.innerHTML='<td>'+sanitize(u.username)+'</td><td>'+sanitize(u.role)+'</td><td><button class="btn" onclick="impersonate(\''+u.username+'\')">Impersonate</button></td>'; ut.appendChild(tr); }); }
    window.removeUser = function(username){ if(!confirm('Remove '+username+'?')) return; let users=JSON.parse(localStorage.getItem('users')||'[]'); users = users.filter(x=>x.username!==username); localStorage.setItem('users',JSON.stringify(users)); log('Removed user '+username); renderUsers(); }
    window.impersonate = function(username){ alert('Impersonation simulated: now viewing as '+username); log('Impersonated '+username); }

    // misc
    $('#logout').onclick = ()=>{ alert('Logged out (simulated)'); log('Admin logged out'); }

    // init
    seed(); renderDashboard(); renderStudents(''); renderCourses(); renderFaculty(); renderExams(); renderDocs(); renderRequests(); renderJobs(); renderBooks(); renderRooms(); renderLogs(); renderUsers();

    // small dynamic: when selecting course for marks update subjects select
    // also set up one-time click handler on student table to route actions (delegation)

    // ensure marks subject updates at start
    if($('#marksCourse').options.length){ const ev = new Event('change'); $('#marksCourse').dispatchEvent(ev); }

    // attach global handler to populate marks subjects when course changes
    $('#marksCourse').addEventListener('change', e=>{ const course=e.target.value; const c=getCourses().find(x=>x.name===course); const sub=$('#marksSubject'); sub.innerHTML=''; (c?.subjects||[]).forEach(s=>{const o=document.createElement('option'); o.value=s; o.textContent=s; sub.appendChild(o); }); });

  </script>

</body>
</html>
