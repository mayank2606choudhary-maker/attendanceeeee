import React, { useState, useEffect } from 'react';
import { Plus, Minus, Edit2, Check, GraduationCap, AlertCircle, Trash2, Sparkles, X, MessageSquare, Loader2, Calendar, Settings, Clock, LogOut, User } from 'lucide-react';

// --- FIREBASE CONFIGURATION ---
// 1. Go to console.firebase.google.com
// 2. Create a project -> Add Web App -> Copy config here
// 3. Enable "Authentication" (Email/Password) and "Firestore Database" in console
import { initializeApp } from 'firebase/app';
import { 
  getAuth, 
  createUserWithEmailAndPassword, 
  signInWithEmailAndPassword, 
  signOut, 
  onAuthStateChanged 
} from 'firebase/auth';
import { 
  getFirestore, 
  doc, 
  setDoc, 
  getDoc 
} from 'firebase/firestore';

// REPLACE THIS WITH YOUR OWN FIREBASE CONFIG FROM THE CONSOLE
const firebaseConfig = {
  apiKey: "AIzaSyChm6JbPXTS5-_q8_z5duMCuSLmsPtSVoA",
  authDomain: "attendance-660e5.firebaseapp.com",
  projectId: "attendance-660e5",
  storageBucket: "attendance-660e5.firebasestorage.app",
  messagingSenderId: "333511390890",
  appId: "1:333511390890:web:80ae69a3bea50aa87f42d3"
};

// Initialize Firebase (Try/Catch handles the preview environment without keys)
let auth, db;
try {
  const app = initializeApp(firebaseConfig);
  auth = getAuth(app);
  db = getFirestore(app);
} catch (e) {
  console.log("Firebase not configured yet.");
}

// --- DATA STRUCTURE ---
const DEFAULT_SUBJECTS = [
  { id: 1, name: 'Business and Corporate Finance', present: 0, absent: 0, maxSessions: 24, history: [] },
  { id: 2, name: 'Business Communication II', present: 0, absent: 0, maxSessions: 12, history: [] },
  { id: 3, name: 'Business Environment', present: 0, absent: 0, maxSessions: 24, history: [] },
  { id: 4, name: 'Data Science and Analytics', present: 0, absent: 0, maxSessions: 16, history: [] },
  { id: 5, name: 'Human Resource Management', present: 0, absent: 0, maxSessions: 24, history: [] },
  { id: 6, name: 'Legal Aspects of Business - II', present: 0, absent: 0, maxSessions: 12, history: [] },
  { id: 7, name: 'Managerial Accounting', present: 0, absent: 0, maxSessions: 24, history: [] },
  { id: 8, name: 'Marketing Management - II', present: 0, absent: 0, maxSessions: 24, history: [] },
  { id: 9, name: 'Supply Chain Management', present: 0, absent: 0, maxSessions: 24, history: [] },
];

export default function AttendanceApp() {
  const [user, setUser] = useState(null);
  const [loading, setLoading] = useState(true);
  
  // App Data State
  const [subjects, setSubjects] = useState(DEFAULT_SUBJECTS);
  
  // UI States
  const [activeModal, setActiveModal] = useState(null);
  const [selectedSubject, setSelectedSubject] = useState(null);
  const [authMode, setAuthMode] = useState('login'); // 'login' or 'signup'
  const [email, setEmail] = useState('');
  const [password, setPassword] = useState('');
  const [authError, setAuthError] = useState('');

  // Edit/History States
  const [editForm, setEditForm] = useState({ name: '', present: 0, total: 0 });
  const [newDate, setNewDate] = useState(new Date().toISOString().split('T')[0]);
  const [newDateStatus, setNewDateStatus] = useState('present');

  // AI State
  const [aiLoading, setAiLoading] = useState(false);
  const [aiResponse, setAiResponse] = useState("");
  const [aiTitle, setAiTitle] = useState("");
  const [showResetConfirm, setShowResetConfirm] = useState(false);

  // --- AUTHENTICATION & SYNC LOGIC ---

  useEffect(() => {
    if (!auth) {
      setLoading(false);
      return;
    }
    const unsubscribe = onAuthStateChanged(auth, async (currentUser) => {
      setUser(currentUser);
      if (currentUser) {
        // Load data from Firestore
        try {
          const docRef = doc(db, "users", currentUser.uid);
          const docSnap = await getDoc(docRef);
          if (docSnap.exists()) {
            setSubjects(docSnap.data().subjects);
          } else {
            // New user: Save default subjects
            await setDoc(docRef, { subjects: DEFAULT_SUBJECTS });
          }
        } catch (error) {
          console.error("Error loading data:", error);
        }
      }
      setLoading(false);
    });
    return () => unsubscribe();
  }, []);

  // Save to Firestore whenever subjects change (Debounced slightly in real apps, direct here)
  const saveToCloud = async (newSubjects) => {
    setSubjects(newSubjects); // Optimistic update
    if (user && db) {
      try {
        await setDoc(doc(db, "users", user.uid), { subjects: newSubjects }, { merge: true });
      } catch (e) {
        console.error("Error saving to cloud", e);
      }
    }
  };

  const handleAuth = async (e) => {
    e.preventDefault();
    setAuthError('');
    if (!auth) {
      setAuthError("Firebase config is missing in the code!");
      return;
    }
    try {
      if (authMode === 'login') {
        await signInWithEmailAndPassword(auth, email, password);
      } else {
        await createUserWithEmailAndPassword(auth, email, password);
      }
    } catch (err) {
      setAuthError(err.message.replace('Firebase: ', ''));
    }
  };

  const handleLogout = () => {
    signOut(auth);
    setSubjects(DEFAULT_SUBJECTS);
  };

  // --- APP LOGIC (ADAPTED FOR CLOUD) ---

  const updateAttendance = (id, type, change) => {
    const newSubjects = subjects.map(sub => {
      if (sub.id !== id) return sub;
      
      const newValue = sub[type] + change;
      if (newValue < 0) return sub;

      let newHistory = [...sub.history];
      if (change > 0) {
        newHistory.unshift({ id: Date.now(), date: new Date().toISOString().split('T')[0], type: type });
      } else {
        const indexToRemove = newHistory.findIndex(h => h.type === type);
        if (indexToRemove !== -1) newHistory.splice(indexToRemove, 1);
      }
      return { ...sub, [type]: newValue, history: newHistory };
    });
    saveToCloud(newSubjects);
  };

  const manualUpdate = () => {
    if (!selectedSubject) return;
    const newAbsent = Math.max(0, parseInt(editForm.total) - parseInt(editForm.present));
    const newSubjects = subjects.map(sub => 
      sub.id === selectedSubject.id 
        ? { ...sub, name: editForm.name, present: parseInt(editForm.present), absent: newAbsent } 
        : sub
    );
    saveToCloud(newSubjects);
    closeModal();
  };

  const addHistoryEntry = () => {
    if (!selectedSubject || !newDate) return;
    const newSubjects = subjects.map(sub => {
      if (sub.id !== selectedSubject.id) return sub;
      const newHistoryItem = { id: Date.now(), date: newDate, type: newDateStatus };
      const updatedSub = { ...sub, history: [newHistoryItem, ...sub.history].sort((a, b) => new Date(b.date) - new Date(a.date)) };
      if (newDateStatus === 'present') updatedSub.present += 1;
      else updatedSub.absent += 1;
      return updatedSub;
    });
    saveToCloud(newSubjects);
  };

  const deleteHistoryEntry = (entryId, type) => {
    const newSubjects = subjects.map(sub => {
      if (sub.id !== selectedSubject.id) return sub;
      const updatedSub = { ...sub, history: sub.history.filter(h => h.id !== entryId) };
      if (type === 'present') updatedSub.present = Math.max(0, updatedSub.present - 1);
      else updatedSub.absent = Math.max(0, updatedSub.absent - 1);
      return updatedSub;
    });
    saveToCloud(newSubjects);
  };

  const handleResetAll = () => {
    saveToCloud(DEFAULT_SUBJECTS);
    setShowResetConfirm(false);
  };

  // --- GEMINI LOGIC (UNCHANGED) ---
  const callGemini = async (prompt, title) => {
    setAiTitle(title);
    setActiveModal('ai');
    setAiLoading(true);
    setAiResponse("");
    const apiKey = ""; 
    const url = `https://generativelanguage.googleapis.com/v1beta/models/gemini-2.5-flash-preview-09-2025:generateContent?key=${apiKey}`;
    try {
      const response = await fetch(url, { method: 'POST', headers: { 'Content-Type': 'application/json' }, body: JSON.stringify({ contents: [{ parts: [{ text: prompt }] }] }) });
      const data = await response.json();
      setAiResponse(data?.candidates?.[0]?.content?.parts?.[0]?.text || "No response generated.");
    } catch (error) { setAiResponse("Could not connect to AI."); } finally { setAiLoading(false); }
  };
  const generateReport = () => {
    const dataSummary = subjects.map(s => `${s.name}: ${Math.round((s.present/(s.present+s.absent||1))*100)}%`).join('\n');
    callGemini(`Analyze attendance:\n${dataSummary}\n\nStrict, short academic advice.`, "Academic Report");
  };
  const generateExcuse = (subjectName) => callGemini(`Write a creative short excuse for missing ${subjectName}.`, `Excuse: ${subjectName}`);

  // --- HELPERS ---
  const calculatePercentage = (p, a) => { const t = p + a; return t === 0 ? 0 : Math.round((p / t) * 100); };
  const getColorClass = (p) => p >= 75 ? "text-green-600 bg-green-100" : p >= 60 ? "text-yellow-600 bg-yellow-100" : "text-red-600 bg-red-100";
  const getBarColor = (p) => p >= 75 ? "bg-green-500" : p >= 60 ? "bg-yellow-500" : "bg-red-500";
  const closeModal = () => { setActiveModal(null); setSelectedSubject(null); };
  const openEditModal = (s) => { setSelectedSubject(s); setEditForm({ name: s.name, present: s.present, total: s.present + s.absent }); setActiveModal('edit'); };
  const openHistoryModal = (s) => { setSelectedSubject(s); setNewDate(new Date().toISOString().split('T')[0]); setActiveModal('history'); };
  const stats = { 
    totalPresent: subjects.reduce((a, c) => a + c.present, 0), 
    totalAbsent: subjects.reduce((a, c) => a + c.absent, 0),
    percentage: calculatePercentage(subjects.reduce((a, c) => a + c.present, 0), subjects.reduce((a, c) => a + c.absent, 0))
  };

  // --- AUTH SCREEN ---
  if (!user && !loading) {
    return (
      <div className="min-h-screen bg-gray-50 flex flex-col items-center justify-center p-4">
        <div className="bg-white rounded-2xl shadow-xl w-full max-w-md p-8">
          <div className="flex justify-center mb-6">
            <div className="bg-indigo-100 p-3 rounded-full">
              <GraduationCap className="w-10 h-10 text-indigo-600" />
            </div>
          </div>
          <h1 className="text-2xl font-bold text-center text-gray-800 mb-2">AttendancePro Cloud</h1>
          <p className="text-center text-gray-500 mb-8">Sign in to sync your attendance across devices.</p>
          
          <form onSubmit={handleAuth} className="space-y-4">
            <div>
              <label className="block text-sm font-medium text-gray-700 mb-1">Email</label>
              <input 
                type="email" 
                required
                className="w-full p-3 border border-gray-300 rounded-lg focus:ring-2 focus:ring-indigo-500 outline-none"
                value={email}
                onChange={e => setEmail(e.target.value)}
              />
            </div>
            <div>
              <label className="block text-sm font-medium text-gray-700 mb-1">Password</label>
              <input 
                type="password" 
                required
                className="w-full p-3 border border-gray-300 rounded-lg focus:ring-2 focus:ring-indigo-500 outline-none"
                value={password}
                onChange={e => setPassword(e.target.value)}
              />
            </div>
            
            {authError && (
              <div className="p-3 bg-red-50 text-red-600 text-sm rounded-lg flex items-center gap-2">
                <AlertCircle size={16} /> {authError}
              </div>
            )}

            <button type="submit" className="w-full bg-indigo-600 text-white py-3 rounded-lg font-bold hover:bg-indigo-700 transition-all">
              {authMode === 'login' ? 'Sign In' : 'Create Account'}
            </button>
          </form>

          <div className="mt-6 text-center text-sm text-gray-600">
            {authMode === 'login' ? "Don't have an account? " : "Already have an account? "}
            <button 
              onClick={() => { setAuthMode(authMode === 'login' ? 'signup' : 'login'); setAuthError(''); }} 
              className="text-indigo-600 font-bold hover:underline"
            >
              {authMode === 'login' ? 'Sign Up' : 'Log In'}
            </button>
          </div>
          
          <div className="mt-8 pt-6 border-t text-center text-xs text-gray-400">
             Note: You must replace the "firebaseConfig" in the code with your own keys for this to work.
          </div>
        </div>
      </div>
    );
  }

  // --- MAIN APP (AUTHENTICATED) ---
  return (
    <div className="min-h-screen bg-gray-50 pb-12 font-sans relative">
      {/* Header */}
      <header className="bg-white border-b sticky top-0 z-10 shadow-sm">
        <div className="max-w-4xl mx-auto px-4 py-4 flex justify-between items-center">
          <div className="flex items-center gap-2">
            <GraduationCap className="w-8 h-8 text-indigo-600" />
            <h1 className="text-xl font-bold text-gray-800 hidden sm:block">Attendance<span className="text-indigo-600">Pro</span></h1>
          </div>
          <div className="flex items-center gap-3">
             <div className="hidden sm:flex items-center gap-2 bg-gray-100 px-3 py-1 rounded-full text-xs font-medium text-gray-600">
               <User size={14} /> {user?.email}
             </div>
            <button onClick={generateReport} className="flex items-center gap-2 bg-indigo-600 hover:bg-indigo-700 text-white px-4 py-2 rounded-full text-sm font-medium transition-all shadow-sm">
              <Sparkles size={16} className="text-yellow-300" />
              <span className="hidden sm:inline">AI Analysis</span>
            </button>
            <button onClick={handleLogout} className="p-2 text-gray-400 hover:bg-red-50 hover:text-red-600 rounded-full transition-colors" title="Log Out">
              <LogOut size={20} />
            </button>
          </div>
        </div>
      </header>

      {/* Main Content */}
      <main className="max-w-4xl mx-auto px-4 py-6">
        <div className="bg-white rounded-xl shadow-sm border border-gray-100 p-6 mb-8 flex flex-col md:flex-row items-center justify-between gap-6">
          <div className="text-center md:text-left">
            <h2 className="text-gray-500 font-medium mb-1">Overall Attendance</h2>
            <div className="text-4xl font-extrabold text-gray-900">{stats.percentage}%</div>
            <p className="text-xs text-gray-400 mt-1">Target: 75%</p>
          </div>
          <div className="flex-1 w-full md:w-auto">
            <div className="h-4 bg-gray-100 rounded-full overflow-hidden w-full">
              <div className={`h-full transition-all duration-500 ${getBarColor(stats.percentage)}`} style={{ width: `${stats.percentage}%` }}></div>
            </div>
            <div className="flex justify-between mt-2 text-sm text-gray-600">
              <span>Present: {stats.totalPresent}</span>
              <span>Absent: {stats.totalAbsent}</span>
            </div>
          </div>
        </div>

        <div className="grid grid-cols-1 md:grid-cols-2 gap-4">
          {subjects.map((subject) => {
            const pct = calculatePercentage(subject.present, subject.absent);
            return (
              <div key={subject.id} className="bg-white rounded-xl p-5 shadow-sm border border-gray-100 hover:shadow-md transition-shadow group relative">
                <div className="flex justify-between items-start mb-4">
                  <div>
                    <h3 className="font-bold text-gray-800 text-lg truncate w-40 sm:w-auto">{subject.name}</h3>
                    <div className="flex gap-2 mt-1">
                      <button onClick={() => openHistoryModal(subject)} className="text-xs flex items-center gap-1 text-gray-400 hover:text-indigo-600 transition-colors"><Calendar size={12} /> History</button>
                      <button onClick={() => generateExcuse(subject.name)} className="text-xs flex items-center gap-1 text-gray-400 hover:text-purple-600 transition-colors"><MessageSquare size={12} /> Excuse</button>
                    </div>
                  </div>
                  <button onClick={() => openEditModal(subject)} className="p-2 text-gray-300 hover:bg-gray-50 hover:text-gray-600 rounded-full transition-colors"><Settings size={18} /></button>
                </div>
                <div className="mb-4">
                  <div className="flex justify-between text-sm mb-1">
                    <span className={`font-bold px-2 py-0.5 rounded text-xs ${getColorClass(pct)}`}>{pct}%</span>
                    <span className="text-gray-400 text-xs">
                      {subject.present + subject.absent} / {subject.maxSessions || '?'} Sessions
                    </span>
                  </div>
                  <div className="h-2 bg-gray-100 rounded-full overflow-hidden">
                    <div className={`h-full transition-all duration-300 ${getBarColor(pct)}`} style={{ width: `${pct}%` }}></div>
                  </div>
                </div>
                <div className="grid grid-cols-2 gap-3">
                  <div className="bg-green-50 rounded-lg p-2 border border-green-100">
                    <div className="text-xs text-green-700 font-semibold mb-2 text-center">PRESENT</div>
                    <div className="flex items-center justify-between bg-white rounded-md shadow-sm">
                      <button onClick={() => updateAttendance(subject.id, 'present', -1)} className="p-2 text-gray-400 hover:text-red-500 hover:bg-gray-50 rounded-l"><Minus size={14} /></button>
                      <span className="font-bold text-gray-800 w-6 text-center">{subject.present}</span>
                      <button onClick={() => updateAttendance(subject.id, 'present', 1)} className="p-2 text-green-600 hover:bg-green-100 rounded-r"><Plus size={16} /></button>
                    </div>
                  </div>
                  <div className="bg-red-50 rounded-lg p-2 border border-red-100">
                    <div className="text-xs text-red-700 font-semibold mb-2 text-center">ABSENT</div>
                    <div className="flex items-center justify-between bg-white rounded-md shadow-sm">
                      <button onClick={() => updateAttendance(subject.id, 'absent', -1)} className="p-2 text-gray-400 hover:text-red-500 hover:bg-gray-50 rounded-l"><Minus size={14} /></button>
                      <span className="font-bold text-gray-800 w-6 text-center">{subject.absent}</span>
                      <button onClick={() => updateAttendance(subject.id, 'absent', 1)} className="p-2 text-red-600 hover:bg-red-100 rounded-r"><Plus size={16} /></button>
                    </div>
                  </div>
                </div>
              </div>
            );
          })}
        </div>

        <div className="mt-12 pt-8 border-t border-gray-200 text-center">
          {!showResetConfirm ? (
            <button onClick={() => setShowResetConfirm(true)} className="flex items-center gap-2 text-sm text-gray-400 hover:text-red-600 transition-colors mx-auto"><Trash2 size={16} /> Reset All Data</button>
          ) : (
            <div className="inline-flex gap-2">
              <button onClick={() => setShowResetConfirm(false)} className="px-4 py-2 bg-white border rounded-lg text-sm hover:bg-gray-50">Cancel</button>
              <button onClick={handleResetAll} className="px-4 py-2 bg-red-600 text-white rounded-lg text-sm hover:bg-red-700">Yes, Reset</button>
            </div>
          )}
        </div>

        {/* Footer Credit */}
        <div className="mt-8 text-center text-xs text-indigo-400 font-medium opacity-75">
          Made with ❤️ by lensgrapher
        </div>
      </main>

      {/* MODALS */}
      {activeModal === 'edit' && selectedSubject && (
        <div className="fixed inset-0 z-50 flex items-center justify-center p-4 bg-black/50 backdrop-blur-sm">
          <div className="bg-white rounded-xl shadow-xl w-full max-w-sm overflow-hidden animate-in zoom-in duration-200">
            <div className="bg-gray-50 px-6 py-4 border-b flex justify-between items-center"><h3 className="font-bold text-gray-800">Edit Subject</h3><button onClick={closeModal}><X size={20} className="text-gray-400 hover:text-gray-600"/></button></div>
            <div className="p-6 space-y-4">
              <div><label className="block text-xs font-semibold text-gray-500 mb-1">Subject Name</label><input value={editForm.name} onChange={e => setEditForm({...editForm, name: e.target.value})} className="w-full p-2 border rounded-lg focus:ring-2 focus:ring-indigo-500 outline-none" /></div>
              <div className="grid grid-cols-2 gap-4"><div><label className="block text-xs font-semibold text-gray-500 mb-1">Total Classes</label><input type="number" min="0" value={editForm.total} onChange={e => setEditForm({...editForm, total: e.target.value})} className="w-full p-2 border rounded-lg focus:ring-2 focus:ring-indigo-500 outline-none" /></div><div><label className="block text-xs font-semibold text-gray-500 mb-1">Attended</label><input type="number" min="0" value={editForm.present} onChange={e => setEditForm({...editForm, present: e.target.value})} className="w-full p-2 border rounded-lg focus:ring-2 focus:ring-indigo-500 outline-none" /></div></div>
            </div>
            <div className="p-4 border-t flex justify-end bg-gray-50"><button onClick={manualUpdate} className="bg-indigo-600 text-white px-4 py-2 rounded-lg text-sm font-medium hover:bg-indigo-700">Save Changes</button></div>
          </div>
        </div>
      )}

      {activeModal === 'history' && selectedSubject && (
        <div className="fixed inset-0 z-50 flex items-center justify-center p-4 bg-black/50 backdrop-blur-sm">
          <div className="bg-white rounded-xl shadow-xl w-full max-w-md overflow-hidden animate-in zoom-in duration-200 flex flex-col max-h-[80vh]">
            <div className="bg-indigo-600 px-6 py-4 flex justify-between items-center text-white shrink-0"><div className="flex items-center gap-2"><Calendar size={20} /><h3 className="font-bold">{selectedSubject.name} Log</h3></div><button onClick={closeModal}><X size={20} className="hover:text-indigo-200"/></button></div>
            <div className="p-4 bg-indigo-50 border-b shrink-0"><label className="block text-xs font-bold text-indigo-900 mb-2">Add Past Class</label><div className="flex gap-2"><input type="date" value={newDate} onChange={e => setNewDate(e.target.value)} className="flex-1 p-2 text-sm border rounded-lg outline-none focus:ring-2 focus:ring-indigo-500"/><select value={newDateStatus} onChange={e => setNewDateStatus(e.target.value)} className="p-2 text-sm border rounded-lg outline-none focus:ring-2 focus:ring-indigo-500 bg-white"><option value="present">Present</option><option value="absent">Absent</option></select><button onClick={addHistoryEntry} className="bg-indigo-600 text-white p-2 rounded-lg hover:bg-indigo-700"><Plus size={18} /></button></div></div>
            <div className="overflow-y-auto p-4 flex-1">{selectedSubject.history && selectedSubject.history.length > 0 ? (<div className="space-y-2">{selectedSubject.history.map((entry) => (<div key={entry.id} className="flex items-center justify-between p-3 bg-gray-50 rounded-lg border border-gray-100"><div className="flex items-center gap-3"><div className={`w-2 h-2 rounded-full ${entry.type === 'present' ? 'bg-green-500' : 'bg-red-500'}`}></div><span className="text-sm font-medium text-gray-700">{new Date(entry.date).toLocaleDateString(undefined, { weekday: 'short', month: 'short', day: 'numeric' })}</span></div><div className="flex items-center gap-3"><span className={`text-xs font-bold px-2 py-1 rounded capitalize ${entry.type === 'present' ? 'text-green-700 bg-green-100' : 'text-red-700 bg-red-100'}`}>{entry.type}</span><button onClick={() => deleteHistoryEntry(entry.id, entry.type)} className="text-gray-400 hover:text-red-500"><Trash2 size={14} /></button></div></div>))}</div>) : (<div className="text-center py-12 text-gray-400"><Clock size={48} className="mx-auto mb-2 opacity-20" /><p>No history yet.</p></div>)}</div>
          </div>
        </div>
      )}

      {activeModal === 'ai' && (
        <div className="fixed inset-0 z-50 flex items-center justify-center p-4 bg-black/50 backdrop-blur-sm">
          <div className="bg-white rounded-2xl shadow-2xl w-full max-w-lg overflow-hidden animate-in zoom-in duration-200">
            <div className="bg-indigo-600 p-4 flex justify-between items-center text-white"><div className="flex items-center gap-2"><Sparkles className="w-5 h-5 text-yellow-300" /><h3 className="font-bold">{aiTitle}</h3></div><button onClick={closeModal} className="hover:bg-indigo-700 p-1 rounded transition-colors"><X size={20} /></button></div>
            <div className="p-6 max-h-[60vh] overflow-y-auto">{aiLoading ? (<div className="flex flex-col items-center justify-center py-8 space-y-4"><Loader2 className="w-10 h-10 text-indigo-600 animate-spin" /><p className="text-gray-500 text-sm animate-pulse">Consulting the AI...</p></div>) : (<div className="prose prose-sm max-w-none text-gray-700 whitespace-pre-wrap leading-relaxed">{aiResponse}</div>)}</div>
            <div className="bg-gray-50 p-4 border-t flex justify-end"><button onClick={closeModal} className="px-4 py-2 bg-white border border-gray-300 rounded-lg text-sm font-medium hover:bg-gray-100">Close</button></div>
          </div>
        </div>
      )}
    </div>
  );
}
