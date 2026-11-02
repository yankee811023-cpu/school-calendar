import React, { useState, useEffect, useRef } from 'react';
import { initializeApp } from 'firebase/app';
import { getAuth, signInAnonymously, signInWithCustomToken } from 'firebase/auth';
import { getFirestore, collection, doc, setDoc, onSnapshot } from 'firebase/firestore';
import { Calendar, CheckCircle2, BarChart3, Calculator as CalcIcon, Users, Share2, X } from 'lucide-react';

const appId = typeof __app_id !== 'undefined' ? __app_id : 'default-app-id';
const firebaseConfig = typeof __firebase_config !== 'undefined' ? JSON.parse(__firebase_config) : {};
const initialAuthToken = typeof __initial_auth_token !== 'undefined' ? __initial_auth_token : null;

let app, auth, db;
try {
  app = initializeApp(firebaseConfig);
  auth = getAuth(app);
  db = getFirestore(app);
} catch (e) {
  console.error("Firebase init error", e);
}

export default function App() {
  const [isReady, setIsReady] = useState(false);
  const [user, setUser] = useState('');
  const [userNameInput, setUserNameInput] = useState('');

  // 시뮬레이터 상태
  const [startDate, setStartDate] = useState('2027-03-02'); // 기본 시작일
  const [semester2StartDate, setSemester2StartDate] = useState('2027-08-26'); // 2학기 기본 시작일
  const [selectedOptional, setSelectedOptional] = useState(['2027-05-04']); // 기본 선택
  const [selectedScheduleOption, setSelectedScheduleOption] = useState(98); // 98, 99, 100 중 택 1
  const [scheduleOpinions, setScheduleOpinions] = useState([]); // 수합된 일정 의견들

  const [showShareModal, setShowShareModal] = useState(false);

  const baseHolidays = [
    { date: '2027-05-03', name: '대체공휴일(노동절)' },
    { date: '2027-05-05', name: '어린이날' },
    { date: '2027-05-13', name: '부처님오신날' },
    { date: '2027-07-19', name: '대체공휴일(제헌절)' },
    { date: '2027-08-16', name: '대체공휴일(광복절)' },
    { date: '2027-09-14', name: '추석 연휴' },
    { date: '2027-09-15', name: '추석' },
    { date: '2027-09-16', name: '추석 연휴' },
    { date: '2027-10-04', name: '대체공휴일(개천절)' },
    { date: '2027-10-11', name: '대체공휴일(한글날)' },
    { date: '2027-12-25', name: '크리스마스' },
    { date: '2027-12-27', name: '대체공휴일(크리스마스)' }
  ];

  const optionalHolidaysList = [
    { date: '2027-05-04', name: '5/4 재량휴업일' },
    { date: '2027-05-14', name: '5/14 재량휴업일' },
    { date: '2027-09-13', name: '9/13 재량휴업일' },
    { date: '2027-09-17', name: '9/17 재량휴업일' }
  ];

  const handleToggleOptional = (date) => {
    if (selectedOptional.includes(date)) {
      setSelectedOptional(selectedOptional.filter(d => d !== date));
    } else {
      setSelectedOptional([...selectedOptional, date]);
    }
  };

  // 선택한 학사일정 의견을 제출하는 함수
  const handleSubmitScheduleOpinion = async () => {
    if (!auth?.currentUser || !user) return;
    
    const opRef = doc(db, 'artifacts', appId, 'public', 'data', 'schedule_opinions', user);
    await setDoc(opRef, {
      user: user,
      holidays: selectedOptional,
      days1: selectedScheduleOption,
      semester2Start: semester2StartDate,
      updatedAt: Date.now()
    });
  };

  useEffect(() => {
    const initAuth = async () => {
      try {
        if (initialAuthToken) {
          await signInWithCustomToken(auth, initialAuthToken);
        } else {
          await signInAnonymously(auth);
        }
        setIsReady(true);
      } catch (error) {
        console.error('Firebase Auth Error:', error);
      }
    };
    if (auth) initAuth();
  }, []);

  useEffect(() => {
    if (!isReady || !auth?.currentUser) return;
    
    const opinionsRef = collection(db, 'artifacts', appId, 'public', 'data', 'schedule_opinions');
    const unsubscribeOpinions = onSnapshot(opinionsRef, (snapshot) => {
      const fetchedOps = [];
      snapshot.forEach(docSnap => {
        fetchedOps.push(docSnap.data());
      });
      setScheduleOpinions(fetchedOps);
    }, (error) => {
      console.error("Firestore Opinions Snapshot Error:", error);
    });

    return () => {
      unsubscribeOpinions();
    };
  }, [isReady]);

  // 수업일수 기반 날짜 계산 로직
  const getSchoolEndDate = (startStr, targetDays, opts = selectedOptional) => {
    if (!startStr) return { lastDay: '', vacationStart: '' };
    let currentDate = new Date(startStr);
    let count = 1;

    const activeOptional = optionalHolidaysList.filter(h => opts.includes(h.date));
    const allActiveHolidays = [...baseHolidays, ...activeOptional];
    const holidayDates = allActiveHolidays.map(h => h.date);

    if (currentDate.getDay() === 0 || currentDate.getDay() === 6 || holidayDates.includes(startStr)) {
         count = 0; 
    }

    while(count < targetDays) {
        currentDate.setDate(currentDate.getDate() + 1);
        const dayOfWeek = currentDate.getDay();
        const dateString = currentDate.toISOString().split('T')[0];

        const isWeekend = dayOfWeek === 0 || dayOfWeek === 6;
        const isHoliday = holidayDates.includes(dateString);

        if (!isWeekend && !isHoliday) {
            count++;
        }
    }
    
    const lastSchoolDay = new Date(currentDate);
    currentDate.setDate(currentDate.getDate() + 1);
    
    const days = ['일', '월', '화', '수', '목', '금', '토'];
    const formatDt = (dt) => `${dt.getFullYear()}-${String(dt.getMonth()+1).padStart(2,'0')}-${String(dt.getDate()).padStart(2,'0')} (${days[dt.getDay()]})`;
    
    return {
        lastDay: formatDt(lastSchoolDay),
        vacationStart: formatDt(currentDate)
    };
  };

  const handleLogin = (e) => {
    e.preventDefault();
    if (userNameInput.trim()) {
      setUser(userNameInput.trim());
    }
  };

  if (!user) {
    return (
      <div className="min-h-screen bg-slate-50 flex items-center justify-center p-4">
        <div className="bg-white p-8 rounded-2xl shadow-lg max-w-md w-full">
          <div className="flex justify-center mb-6">
            <div className="bg-blue-100 p-3 rounded-full">
              <Calendar className="w-8 h-8 text-blue-600" />
            </div>
          </div>
          <h1 className="text-2xl font-bold text-center text-slate-800 mb-2">학사일정 의견 수합</h1>
          <p className="text-center text-slate-500 mb-8">선생님의 성함(또는 직책)을 입력해주세요.</p>
          
          <form onSubmit={handleLogin} className="space-y-4">
            <div>
              <input
                type="text"
                value={userNameInput}
                onChange={(e) => setUserNameInput(e.target.value)}
                placeholder="예: 홍길동(교무부)"
                className="w-full px-4 py-3 rounded-lg border border-slate-200 focus:outline-none focus:ring-2 focus:ring-blue-500"
                required
              />
            </div>
            <button
              type="submit"
              className="w-full bg-blue-600 text-white font-semibold py-3 rounded-lg hover:bg-blue-700 transition-colors"
            >
              입장하기
            </button>
          </form>
        </div>
      </div>
    );
  }

  return (
    <div className="min-h-screen bg-slate-50 text-slate-800 font-sans pb-12">
      {/* 헤더 */}
      <header className="bg-white shadow-sm sticky top-0 z-10">
        <div className="max-w-4xl mx-auto px-4 h-16 flex items-center justify-between">
          <div className="flex items-center gap-6">
            <div className="flex items-center gap-2 font-bold text-lg text-blue-700">
              <Calendar className="w-6 h-6" />
              <span>학사일정 시뮬레이터</span>
            </div>
          </div>
          <div className="flex items-center gap-3 sm:gap-4">
            <button
              onClick={() => setShowShareModal(true)}
              className="flex items-center gap-1.5 px-3 py-1.5 bg-blue-50 text-blue-600 hover:bg-blue-100 rounded-lg text-sm font-semibold transition-colors"
            >
              <Share2 className="w-4 h-4" />
              <span className="hidden sm:inline-block">공유 안내</span>
            </button>
            <span className="text-sm font-medium text-slate-600 hidden sm:inline-block">{user} 선생님</span>
            <button 
              onClick={() => setUser('')}
              className="text-xs text-slate-400 hover:text-slate-600 underline"
            >
              변경
            </button>
          </div>
        </div>
      </header>

      <main className="max-w-4xl mx-auto px-4 mt-8">
        <div className="animate-in fade-in slide-in-from-bottom-4 duration-300 space-y-6">
          <div className="bg-white rounded-2xl shadow-sm border border-slate-200 p-6 md:p-8">
            <h2 className="text-2xl font-bold mb-2 flex items-center gap-2">
              <CalcIcon className="w-6 h-6 text-blue-600" />
              학사일정 시뮬레이터 및 의견 제출
            </h2>
            <p className="text-slate-500 mb-8">휴업일을 지정하고 원하는 학기 종료일 버전을 선택해 선생님의 의견을 바로 제출해 주세요.</p>
              
            <div className="grid md:grid-cols-2 gap-8">
              {/* 설정 영역 */}
              <div className="space-y-6">
                <div className="flex flex-col sm:flex-row gap-4">
                  <div className="flex-1">
                    <label className="block text-sm font-semibold text-slate-700 mb-2">1학기 시작일 (고정)</label>
                    <input 
                      type="date" 
                      value={startDate} 
                      readOnly
                      className="w-full px-4 py-2 border border-slate-200 rounded-xl bg-slate-100 text-slate-500 cursor-not-allowed focus:outline-none" 
                    />
                  </div>
                  <div className="flex-1">
                    <label className="block text-sm font-semibold text-slate-700 mb-2">2학기 시작일 (선택)</label>
                    <input 
                      type="date" 
                      value={semester2StartDate} 
                      onChange={e => setSemester2StartDate(e.target.value)} 
                      className="w-full px-4 py-2 border-2 border-blue-500 bg-blue-50 text-blue-900 font-semibold rounded-xl focus:outline-none focus:ring-2 focus:ring-blue-400 transition-colors cursor-pointer" 
                    />
                  </div>
                </div>
                
                <div>
                  <label className="block text-sm font-semibold text-slate-700 mb-2">기본 공휴일 (자동 포함)</label>
                  <div className="flex flex-wrap gap-2 mb-6">
                    {baseHolidays.map(h => (
                      <span key={h.date} className="bg-slate-100 text-slate-600 text-xs px-2.5 py-1.5 rounded-lg border border-slate-200">
                        {h.name} ({h.date.substring(5).replace('-', '/')})
                      </span>
                    ))}
                  </div>

                  <label className="block text-sm font-semibold text-slate-700 mb-2">재량휴업일 희망 지정 (클릭하여 선택)</label>
                  <div className="grid grid-cols-1 sm:grid-cols-2 gap-3">
                    {optionalHolidaysList.map(h => {
                      const isSelected = selectedOptional.includes(h.date);
                      return (
                        <button
                          key={h.date}
                          onClick={() => handleToggleOptional(h.date)}
                          className={`flex items-center gap-3 p-3 rounded-xl border-2 transition-all text-left ${isSelected ? 'border-blue-500 bg-blue-50' : 'border-slate-200 bg-white hover:border-slate-300'}`}
                        >
                          <div className={`w-5 h-5 flex-shrink-0 rounded flex items-center justify-center ${isSelected ? 'bg-blue-600' : 'border-2 border-slate-300'}`}>
                            {isSelected && <CheckCircle2 className="w-4 h-4 text-white" />}
                          </div>
                          <div>
                            <div className={`font-bold text-sm ${isSelected ? 'text-blue-900' : 'text-slate-700'}`}>{h.name}</div>
                          </div>
                        </button>
                      );
                    })}
                  </div>
                </div>
              </div>

              {/* 결과 및 제출 영역 */}
              <div className="bg-blue-50/50 p-6 rounded-2xl border border-blue-100 h-fit">
                <h3 className="font-bold text-lg text-blue-900 mb-4">수업일수 선택 (총 수업일수 190일 기준)</h3>
                <div className="space-y-3 mb-6">
                  {[98, 99, 100].map(days1 => {
                    const days2 = 190 - days1;
                    const result1 = getSchoolEndDate(startDate, days1, selectedOptional);
                    const result2 = getSchoolEndDate(semester2StartDate, days2, selectedOptional);
                    const isSelected = selectedScheduleOption === days1;
                    
                    return (
                      <div 
                        key={days1}
                        onClick={() => setSelectedScheduleOption(days1)}
                        className={`p-4 rounded-xl cursor-pointer transition-all border-2 flex flex-col md:flex-row gap-4 divide-y md:divide-y-0 md:divide-x relative ${isSelected ? 'border-blue-500 bg-white shadow-md ring-1 ring-blue-500' : 'border-slate-200 bg-white/60 hover:bg-white hover:border-blue-300 divide-slate-100'}`}
                      >
                        {isSelected && <CheckCircle2 className="absolute -left-2 -top-2 w-6 h-6 text-blue-500 bg-white rounded-full z-10" />}
                        <div className="flex-1 flex justify-between items-center pb-3 md:pb-0 md:pr-4">
                          <div className="font-bold text-slate-800 text-lg pl-2">1학기 {days1}일</div>
                          <div className="text-right">
                            <div className="font-bold text-blue-700">{result1.lastDay}</div>
                            <div className="text-xs text-slate-500 mt-0.5">여름방학: {result1.vacationStart}</div>
                          </div>
                        </div>
                        <div className="flex-1 flex justify-between items-center pt-3 md:pt-0 md:pl-4">
                          <div className="font-bold text-slate-800 text-lg">2학기 {days2}일</div>
                          <div className="text-right">
                            <div className="font-bold text-blue-700">{result2.lastDay}</div>
                            <div className="text-xs text-slate-500 mt-0.5">겨울방학: {result2.vacationStart}</div>
                          </div>
                        </div>
                      </div>
                    )
                  })}
                </div>
                
                {(() => {
                  const hasSubmitted = scheduleOpinions.some(op => op.user === user);
                  return (
                    <button 
                      onClick={handleSubmitScheduleOpinion}
                      className={`w-full font-bold py-3.5 rounded-xl transition-colors shadow-sm flex items-center justify-center gap-2 ${hasSubmitted ? 'bg-slate-700 hover:bg-slate-800 text-white' : 'bg-blue-600 hover:bg-blue-700 text-white'}`}
                    >
                      <CheckCircle2 className="w-5 h-5" />
                      {hasSubmitted ? '선택한 내용으로 내 의견 수정하기' : '선택한 내용으로 의견 제출하기'}
                    </button>
                  );
                })()}
              </div>
            </div>
            
            {/* 실시간 수합 결과 통계 */}
            {scheduleOpinions.length > 0 && (
              <div className="mt-10 pt-8 border-t border-slate-200">
                <h3 className="text-xl font-bold mb-6 flex items-center gap-2">
                  <BarChart3 className="w-6 h-6 text-blue-600" />
                  현재 학사일정 의견 수합 결과 (총 {scheduleOpinions.length}명 참여)
                </h3>
                
                <div className="grid md:grid-cols-2 gap-8">
                  <div className="bg-slate-50 p-6 rounded-2xl border border-slate-100">
                    <h4 className="font-bold text-slate-700 mb-4">수업일수 선호도</h4>
                    <div className="space-y-4">
                      {[98, 99, 100].map(days => {
                        const count = scheduleOpinions.filter(op => op.days1 === days).length;
                        const percent = Math.round((count / scheduleOpinions.length) * 100) || 0;
                        const days2 = 190 - days;
                        return (
                          <div key={days}>
                            <div className="flex justify-between text-sm mb-1">
                              <span className="font-medium text-slate-700">1학기 {days}일, 2학기 {days2}일</span>
                              <span className="font-bold text-slate-900">{percent}% ({count}명)</span>
                            </div>
                            <div className="w-full bg-slate-200 rounded-full h-2.5">
                              <div className="bg-blue-500 h-2.5 rounded-full" style={{ width: `${percent}%` }}></div>
                            </div>
                          </div>
                        );
                      })}
                    </div>
                  </div>

                  <div className="bg-slate-50 p-6 rounded-2xl border border-slate-100">
                    <h4 className="font-bold text-slate-700 mb-4">재량휴업일 희망 일자</h4>
                    <div className="space-y-4">
                      {optionalHolidaysList.map(holiday => {
                        const count = scheduleOpinions.filter(op => (op.holidays || []).includes(holiday.date)).length;
                        const percent = Math.round((count / scheduleOpinions.length) * 100) || 0;
                        return (
                          <div key={holiday.date}>
                            <div className="flex justify-between text-sm mb-1">
                              <span className="font-medium text-slate-700">{holiday.name}</span>
                              <span className="font-bold text-slate-900">{percent}% ({count}명)</span>
                            </div>
                            <div className="w-full bg-slate-200 rounded-full h-2.5">
                              <div className="bg-emerald-500 h-2.5 rounded-full" style={{ width: `${percent}%` }}></div>
                            </div>
                          </div>
                        );
                      })}
                    </div>
                  </div>
                </div>

                {/* 개별 의견 확인 영역 */}
                <div className="mt-12 pt-8 border-t border-slate-200">
                  <h3 className="text-xl font-bold mb-6 flex items-center gap-2">
                    <Users className="w-6 h-6 text-blue-600" />
                    선생님별 상세 의견 목록
                  </h3>
                  <div className="overflow-x-auto bg-white rounded-xl border border-slate-200 shadow-sm">
                    <table className="w-full text-sm text-left text-slate-600 whitespace-nowrap">
                      <thead className="text-xs text-slate-700 uppercase bg-slate-50 border-b border-slate-200">
                        <tr>
                          <th scope="col" className="px-6 py-4 font-bold">성함(직책)</th>
                          <th scope="col" className="px-6 py-4 font-bold">재량휴업일 희망 지정</th>
                          <th scope="col" className="px-6 py-4 font-bold">2학기 시작일</th>
                          <th scope="col" className="px-6 py-4 font-bold">학기 종료일 (1/2학기 수업일수)</th>
                        </tr>
                      </thead>
                      <tbody className="divide-y divide-slate-100">
                        {scheduleOpinions.map((op) => {
                          const days1 = op.days1 || 98;
                          const days2 = 190 - days1;
                          const s2Start = op.semester2Start || '2027-08-26';
                          const opts = op.holidays || [];
                          
                          const res1 = getSchoolEndDate(startDate, days1, opts);
                          const res2 = getSchoolEndDate(s2Start, days2, opts);
                          
                          return (
                            <tr key={op.user} className="hover:bg-slate-50 transition-colors">
                              <td className="px-6 py-4 font-bold text-slate-800">{op.user}</td>
                              <td className="px-6 py-4">
                                <div className="flex gap-1.5">
                                  {optionalHolidaysList.map(h => {
                                    const isSelected = opts.includes(h.date);
                                    return (
                                      <span key={h.date} className={`px-2 py-1 text-xs rounded-md ${isSelected ? 'bg-blue-100 text-blue-800 font-semibold' : 'bg-slate-50 text-slate-400 border border-slate-100'}`}>
                                        {h.name.split(' ')[0]}
                                      </span>
                                    )
                                  })}
                                </div>
                              </td>
                              <td className="px-6 py-4 font-medium text-slate-700">{s2Start}</td>
                              <td className="px-6 py-4">
                                <div className="flex flex-col gap-1 text-xs">
                                  <div><span className="font-semibold text-slate-700 inline-block w-[70px]">1학기 {days1}일</span> <span className="text-blue-700 font-medium">{res1.lastDay}</span></div>
                                  <div><span className="font-semibold text-slate-700 inline-block w-[70px]">2학기 {days2}일</span> <span className="text-blue-700 font-medium">{res2.lastDay}</span></div>
                                </div>
                              </td>
                            </tr>
                          );
                        })}
                      </tbody>
                    </table>
                  </div>
                </div>
              </div>
            )}
          </div>
        </div>
      </main>

      {/* 공유하기 모달 */}
      {showShareModal && (
        <div className="fixed inset-0 bg-slate-900/50 flex items-center justify-center p-4 z-50">
          <div className="bg-white rounded-2xl p-6 max-w-md w-full shadow-xl">
            <div className="flex justify-between items-center mb-4">
              <h3 className="text-lg font-bold text-slate-800 flex items-center gap-2">
                <Share2 className="w-5 h-5 text-blue-600" />
                공유 안내
              </h3>
              <button onClick={() => setShowShareModal(false)} className="text-slate-400 hover:text-slate-600 transition-colors">
                <X className="w-5 h-5" />
              </button>
            </div>
            
            <p className="text-sm text-slate-700 mb-4 font-medium leading-relaxed">
              GitHub 및 Vercel을 이용하여 정식 링크를 생성하시면 다른 선생님들께 바로 공유하실 수 있습니다.
            </p>
            
            <button 
              onClick={() => setShowShareModal(false)}
              className="w-full mt-4 bg-slate-800 text-white font-bold py-3 rounded-xl hover:bg-slate-900 transition-colors"
            >
              닫기
            </button>
          </div>
        </div>
      )}
    </div>
  );
}
