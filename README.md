
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Waymo Depot Report Generator | Futuristic Edition</title>
    <!-- Load Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Outfit:wght@300;400;600;800&display=swap" rel="stylesheet">
    <style>
        :root {
            --waymo-blue: #0052ff;
            --waymo-turquoise: #00f5ff;
            --waymo-dark: #0a0e14;
            --waymo-card: rgba(255, 255, 255, 0.03);
            --metallic-border: rgba(255, 255, 255, 0.1);
            --turquoise-glow: rgba(0, 245, 255, 0.3);
        }

        body {
            font-family: 'Outfit', sans-serif;
            background: radial-gradient(circle at top right, #0d1b2a, #0a0e14);
            color: #e2e8f0;
            min-height: 100vh;
        }

        .glass-panel {
            background: var(--waymo-card);
            backdrop-filter: blur(16px);
            border: 1px solid var(--metallic-border);
            box-shadow: 0 8px 32px 0 rgba(0, 0, 0, 0.6);
        }

        .metallic-header {
            background: linear-gradient(135deg, rgba(255,255,255,0.05) 0%, rgba(255,255,255,0) 100%);
            border-bottom: 1px solid var(--metallic-border);
            position: relative;
        }

        .metallic-header::after {
            content: '';
            position: absolute;
            bottom: -1px;
            left: 0;
            width: 100%;
            height: 1px;
            background: linear-gradient(90deg, transparent, var(--waymo-turquoise), transparent);
        }

        .input-glow:focus {
            box-shadow: 0 0 15px var(--turquoise-glow);
            border-color: var(--waymo-turquoise) !important;
        }

        .btn-futuristic {
            background: linear-gradient(90deg, #0052ff 0%, #00f5ff 100%);
            transition: all 0.4s cubic-bezier(0.175, 0.885, 0.32, 1.275);
            text-transform: uppercase;
            letter-spacing: 2px;
        }

        .btn-futuristic:hover {
            filter: brightness(1.2);
            box-shadow: 0 0 25px rgba(0, 245, 255, 0.5);
            transform: translateY(-2px);
        }

        .dynamic-list-container {
            border-left: 2px solid var(--waymo-turquoise);
            background: linear-gradient(90deg, rgba(0, 245, 255, 0.05) 0%, transparent 100%);
            transition: all 0.3s ease;
        }
        
        .dynamic-list-container:hover {
            background: linear-gradient(90deg, rgba(0, 245, 255, 0.08) 0%, transparent 100%);
            border-left-color: #ffffff;
        }

        input, select, textarea {
            background: rgba(0, 0, 0, 0.3) !important;
            border: 1px solid rgba(255, 255, 255, 0.1) !important;
            color: white !important;
            transition: all 0.2s ease !important;
        }

        input::placeholder {
            color: rgba(255, 255, 255, 0.2);
        }

        .accent-text-turquoise {
            color: var(--waymo-turquoise);
            text-shadow: 0 0 10px var(--turquoise-glow);
        }

        /* Custom scrollbar */
        ::-webkit-scrollbar {
            width: 6px;
        }
        ::-webkit-scrollbar-track {
            background: var(--waymo-dark);
        }
        ::-webkit-scrollbar-thumb {
            background: var(--waymo-turquoise);
            border-radius: 10px;
        }
    </style>
</head>
<body class="p-4 sm:p-8">

    <div id="app" class="max-w-6xl mx-auto glass-panel rounded-3xl overflow-hidden transition-all duration-500">

        <!-- Futuristic Header -->
        <header class="metallic-header p-10 text-center relative">
            <div class="absolute top-0 left-1/2 -translate-x-1/2 w-48 h-1 bg-cyan-400 rounded-full blur-sm"></div>
            <h1 class="text-4xl sm:text-6xl font-extrabold bg-clip-text text-transparent bg-gradient-to-r from-white via-cyan-300 to-blue-500 tracking-tighter">
                Waymo Depot Report
            </h1>
            <p id="timestamp" class="text-cyan-400/50 font-light mt-4 tracking-widest text-xs uppercase italic"></p>
        </header>

        <div class="p-6 sm:p-12 space-y-16">
            
            <!-- Basic Details and Headcount -->
            <section class="grid grid-cols-1 lg:grid-cols-2 gap-10">
                <!-- Location & Shift -->
                <div class="space-y-8">
                    <div class="grid grid-cols-1 sm:grid-cols-2 gap-6">
                        <div class="input-group">
                            <label class="block text-[10px] font-black accent-text-turquoise uppercase mb-3 tracking-[0.2em]">Deployment Site</label>
                            <select id="location" onchange="toggleOtherLocation()" class="w-full p-4 rounded-xl input-glow outline-none transition-all">
                                <option value="Miami">Miami</option>
                                <option value="Orlando">Orlando</option>
                                <option value="Detroit">Detroit</option>
                                <option value="Washington D.C.">Washington D.C.</option>
                                <option value="Baltimore">Baltimore</option>
                                <option value="Tampa">Tampa</option>
                                <option value="Jersey City">Jersey City</option>
                                <option value="Boston">Boston</option>
                                <option value="Charlotte">Charlotte</option>
                                <option value="New York City">New York City</option>
                                <option value="Other">Other...</option>
                            </select>
                            <input type="text" id="otherLocation" placeholder="Specify Mission City" class="hidden mt-4 w-full p-4 rounded-xl input-glow outline-none transition-all border-cyan-500/50">
                        </div>

                        <div class="input-group">
                            <label class="block text-[10px] font-black accent-text-turquoise uppercase mb-3 tracking-[0.2em]">Operational Shift</label>
                            <select id="shift" class="w-full p-4 rounded-xl input-glow outline-none transition-all">
                                <option value="12:00 AM - 9:00 AM">12:00 AM - 9:00 AM</option>
                                <option value="4:00 AM - 1:00 PM">4:00 AM - 1:00 PM</option>
                                <option value="8:00 AM - 5:00 PM">8:00 AM - 5:00 PM</option>
                                <option value="12:00 PM - 9:00 PM">12:00 PM - 9:00 PM</option>
                                <option value="4:00 PM - 1:00 AM">4:00 PM - 1:00 AM</option>
                                <option value="8:00 PM - 5:00 AM">8:00 PM - 5:00 AM</option>
                            </select>
                        </div>
                    </div>
                </div>

                <!-- Headcounts -->
                <div class="grid grid-cols-3 gap-6">
                    <div class="input-group">
                        <label class="block text-[10px] font-bold text-gray-500 uppercase mb-3 tracking-widest">Required</label>
                        <input type="number" id="requiredHC" class="w-full p-4 rounded-xl outline-none border-white/5" value="0">
                    </div>
                    <div class="input-group">
                        <label class="block text-[10px] font-bold text-gray-500 uppercase mb-3 tracking-widest">Planned</label>
                        <input type="number" id="plannedHC" class="w-full p-4 rounded-xl outline-none border-white/5" value="0">
                    </div>
                    <div class="input-group">
                        <label class="block text-[10px] font-bold text-gray-400 uppercase mb-3 tracking-widest">Manual Actual</label>
                        <input type="number" id="actualHC" class="w-full p-4 rounded-xl outline-none border-cyan-500/20 input-glow" value="0">
                    </div>
                </div>
            </section>

            <!-- Status Warnings -->
            <div id="headcountWarning" class="bg-red-500/10 border border-red-500/30 text-red-400 p-5 rounded-2xl text-xs text-center hidden font-bold tracking-wider"></div>

            <!-- Summary Modules -->
            <section class="grid grid-cols-1 sm:grid-cols-2 gap-8">
                <div class="glass-panel p-8 rounded-3xl flex justify-between items-center border-l-4 border-l-cyan-400 group hover:bg-white/5 transition-colors">
                    <span class="text-cyan-100 font-bold uppercase tracking-widest text-xs">Total Attending</span>
                    <span id="totalAttending" class="text-5xl font-black text-cyan-400 drop-shadow-[0_0_10px_rgba(0,245,255,0.4)]">0</span>
                </div>
                <div class="glass-panel p-8 rounded-3xl flex justify-between items-center border-l-4 border-l-red-500 group hover:bg-white/5 transition-colors">
                    <span class="text-red-200 font-bold uppercase tracking-widest text-xs">Total Absences</span>
                    <span id="totalAbsences" class="text-5xl font-black text-red-500 drop-shadow-[0_0_10px_rgba(239,68,68,0.4)]">0</span>
                </div>
            </section>

            <div class="grid grid-cols-1 xl:grid-cols-2 gap-16">

                <!-- Column 1: Personnel -->
                <div class="space-y-10">
                    <h2 class="text-2xl font-black text-white flex items-center gap-4 italic tracking-tighter">
                        <span class="w-3 h-8 bg-cyan-400 rounded-sm skew-x-[-15deg]"></span>
                        MISSION LOGISTICS
                    </h2>

                    <div class="grid grid-cols-1 gap-8">
                        <!-- Mission Sections -->
                        <div class="dynamic-list-container p-6 rounded-r-2xl">
                            <h3 class="text-xs font-black text-cyan-400 mb-4 uppercase tracking-[0.3em]">Mission MDC</h3>
                            <div id="missionMDC_inputs" class="space-y-3 mb-4"></div>
                            <button onclick="addInput('missionMDC', 'text')" class="text-[10px] font-bold text-cyan-200/60 hover:text-cyan-400 transition-all uppercase tracking-widest">+ Add Driver</button>
                        </div>

                        <div class="dynamic-list-container p-6 rounded-r-2xl">
                            <h3 class="text-xs font-black text-cyan-400 mb-4 uppercase tracking-[0.3em]">Mission Mapping</h3>
                            <div id="missionMapping_inputs" class="space-y-3 mb-4"></div>
                            <button onclick="addInput('missionMapping', 'text')" class="text-[10px] font-bold text-cyan-200/60 hover:text-cyan-400 transition-all uppercase tracking-widest">+ Add Driver</button>
                        </div>

                        <div class="dynamic-list-container p-6 rounded-r-2xl">
                            <h3 class="text-xs font-black text-cyan-400 mb-4 uppercase tracking-[0.3em]">Mission ABD</h3>
                            <div id="missionABD_inputs" class="space-y-3 mb-4"></div>
                            <button onclick="addInput('missionABD', 'text')" class="text-[10px] font-bold text-cyan-200/60 hover:text-cyan-400 transition-all uppercase tracking-widest">+ Add Driver</button>
                        </div>

                        <div class="dynamic-list-container p-6 rounded-r-2xl">
                            <h3 class="text-xs font-black text-cyan-400 mb-4 uppercase tracking-[0.3em]">Mission Scouting</h3>
                            <div id="scouting_inputs" class="space-y-3 mb-4"></div>
                            <button onclick="addInput('scouting', 'text')" class="text-[10px] font-bold text-cyan-200/60 hover:text-cyan-400 transition-all uppercase tracking-widest">+ Add Driver</button>
                        </div>

                        <div class="grid grid-cols-1 sm:grid-cols-2 gap-6">
                            <div class="dynamic-list-container p-6 rounded-r-2xl">
                                <h3 class="text-xs font-bold text-gray-500 mb-4 uppercase tracking-widest">Depot Ops</h3>
                                <div id="depot_inputs" class="space-y-3 mb-4"></div>
                                <button onclick="addInput('depot', 'text')" class="text-[10px] font-bold text-gray-600 hover:text-gray-300 transition uppercase tracking-widest">+ Add Personnel</button>
                            </div>
                            <div class="dynamic-list-container p-6 rounded-r-2xl">
                                <h3 class="text-xs font-bold text-gray-500 mb-4 uppercase tracking-widest">Trainers</h3>
                                <div id="trainers_inputs" class="space-y-3 mb-4"></div>
                                <button onclick="addInput('trainers', 'text')" class="text-[10px] font-bold text-gray-600 hover:text-gray-300 transition uppercase tracking-widest">+ Add Personnel</button>
                            </div>
                        </div>

                        <div class="grid grid-cols-1 sm:grid-cols-2 gap-6">
                            <div class="dynamic-list-container p-6 rounded-r-2xl">
                                <h3 class="text-xs font-bold text-gray-500 mb-4 uppercase tracking-widest">Trainees</h3>
                                <div id="trainees_inputs" class="space-y-3 mb-4"></div>
                                <button onclick="addInput('trainees', 'text')" class="text-[10px] font-bold text-gray-600 hover:text-gray-300 transition uppercase tracking-widest">+ Add Personnel</button>
                            </div>
                            <div class="dynamic-list-container p-6 rounded-r-2xl">
                                <h3 class="text-xs font-bold text-gray-500 mb-4 uppercase tracking-widest">Buffer</h3>
                                <div id="buffer_inputs" class="space-y-3 mb-4"></div>
                                <button onclick="addInput('buffer', 'text')" class="text-[10px] font-bold text-gray-600 hover:text-gray-300 transition uppercase tracking-widest">+ Add Personnel</button>
                            </div>
                        </div>
                    </div>
                </div>

                <!-- Column 2: Exceptions -->
                <div class="space-y-10">
                    <h2 class="text-2xl font-black text-white flex items-center gap-4 italic tracking-tighter">
                        <span class="w-3 h-8 bg-red-500 rounded-sm skew-x-[-15deg]"></span>
                        ABSENCES AND EXCEPTIONS
                    </h2>

                    <div class="grid grid-cols-1 gap-8">
                        <div class="dynamic-list-container p-6 rounded-r-2xl border-l-red-500/50">
                            <h3 class="text-xs font-black text-red-400 mb-4 uppercase tracking-[0.3em]">Absence Log</h3>
                            <div id="absent_inputs" class="space-y-3 mb-4"></div>
                            <button onclick="addInput('absent', 'text')" class="text-[10px] font-bold text-red-300/50 hover:text-red-400 transition-all uppercase tracking-widest">+ Add Absent</button>
                        </div>

                        <div class="dynamic-list-container p-6 rounded-r-2xl border-l-red-500/50">
                            <h3 class="text-xs font-black text-red-400 mb-4 uppercase tracking-[0.3em]">No Call No Show</h3>
                            <div id="ncns_inputs" class="space-y-3 mb-4"></div>
                            <button onclick="addInput('ncns', 'text')" class="text-[10px] font-bold text-red-300/50 hover:text-red-400 transition-all uppercase tracking-widest">+ Add NCNS</button>
                        </div>

                        <div class="dynamic-list-container p-6 rounded-r-2xl border-l-orange-500/50">
                            <h3 class="text-xs font-black text-orange-400 mb-4 uppercase tracking-[0.3em]">Failed BAC/Alert Meter</h3>
                            <div id="failedBAC_inputs" class="space-y-3 mb-4"></div>
                            <button onclick="addInput('failedBAC', 'text')" class="text-[10px] font-bold text-orange-300/50 hover:text-orange-400 transition-all uppercase tracking-widest">+ Add Record</button>
                        </div>

                        <div class="grid grid-cols-1 sm:grid-cols-2 gap-6">
                            <div class="dynamic-list-container p-6 rounded-r-2xl border-l-cyan-400/50">
                                <h3 class="text-[10px] font-black text-cyan-300 mb-4 uppercase tracking-widest">Late Arrival</h3>
                                <div id="late_inputs" class="space-y-3 mb-4"></div>
                                <button onclick="addInput('late', 'text-time')" class="text-[10px] font-bold text-cyan-400/40 hover:text-cyan-400 transition uppercase tracking-widest">+ Add</button>
                            </div>
                            <div class="dynamic-list-container p-6 rounded-r-2xl border-l-cyan-400/50">
                                <h3 class="text-[10px] font-black text-cyan-300 mb-4 uppercase tracking-widest">Early Departure</h3>
                                <div id="earlyOut_inputs" class="space-y-3 mb-4"></div>
                                <button onclick="addInput('earlyOut', 'text-time')" class="text-[10px] font-bold text-cyan-400/40 hover:text-cyan-400 transition uppercase tracking-widest">+ Add</button>
                            </div>
                        </div>

                        <div class="dynamic-list-container p-6 rounded-r-2xl border-l-purple-500/50">
                            <h3 class="text-xs font-black text-purple-400 mb-4 uppercase tracking-[0.3em]">Road Events / Rescues</h3>
                            <div class="grid grid-cols-1 sm:grid-cols-2 gap-6">
                                <div>
                                    <div id="roadEvents_inputs" class="space-y-3 mb-3"></div>
                                    <button onclick="addInput('roadEvents', 'text')" class="text-[10px] font-bold text-purple-300/40 hover:text-purple-400 transition uppercase tracking-widest">+ Road Event</button>
                                </div>
                                <div>
                                    <div id="rescues_inputs" class="space-y-3 mb-3"></div>
                                    <button onclick="addInput('rescues', 'text-time')" class="text-[10px] font-bold text-purple-300/40 hover:text-purple-400 transition uppercase tracking-widest">+ Rescue</button>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>

            <!-- Vehicle Metrics -->
            <section class="glass-panel p-10 rounded-[2.5rem] space-y-10 border-t-2 border-t-cyan-400/20">
                <h2 class="text-2xl font-black text-white flex items-center gap-4 italic tracking-tighter">
                    <span class="w-3 h-8 bg-cyan-400 rounded-sm skew-x-[-15deg]"></span>
                    FLEET METRICS
                </h2>
                
                <div class="grid grid-cols-1 md:grid-cols-2 gap-10">
                    <div class="grid grid-cols-2 gap-6">
                        <div class="input-group">
                            <label class="block text-[10px] font-bold text-gray-500 uppercase mb-3 tracking-widest">First Car Out</label>
                            <input type="time" id="firstCar" class="w-full p-4 rounded-xl outline-none input-glow">
                        </div>
                        <div class="input-group">
                            <label class="block text-[10px] font-bold text-gray-500 uppercase mb-3 tracking-widest">Last Car Out</label>
                            <input type="time" id="lastCar" class="w-full p-4 rounded-xl outline-none input-glow">
                        </div>
                    </div>
                    <div class="grid grid-cols-2 sm:grid-cols-4 gap-4">
                        <div class="text-center">
                            <label class="block text-[9px] font-black text-gray-500 mb-2 uppercase tracking-tighter">Total</label>
                            <input type="number" id="totalAVs" class="w-full p-3 rounded-xl text-center" value="0">
                        </div>
                        <div class="text-center">
                            <label class="block text-[9px] font-black text-red-500 mb-2 uppercase tracking-tighter">Down</label>
                            <input type="number" id="downAVs" class="w-full p-3 rounded-xl text-center" value="0">
                        </div>
                        <div class="text-center">
                            <label class="block text-[9px] font-black text-cyan-400 mb-2 uppercase tracking-tighter">Ready</label>
                            <input type="number" id="availableAVs" class="w-full p-3 rounded-xl text-center" value="0">
                        </div>
                        <div class="text-center">
                            <label class="block text-[9px] font-black text-blue-400 mb-2 uppercase tracking-tighter">Train</label>
                            <input type="number" id="trainingAVs" class="w-full p-3 rounded-xl text-center" value="0">
                        </div>
                    </div>
                </div>
            </section>

            <!-- Notes -->
            <section class="space-y-6">
                <h2 class="text-2xl font-black text-white flex items-center gap-4 uppercase tracking-tighter italic">
                    <span class="w-2 h-6 bg-cyan-400 rounded-full blur-[2px]"></span>
                    Shift Intelligence
                </h2>
                <textarea id="notes" rows="5" class="w-full p-6 rounded-3xl outline-none focus:ring-2 focus:ring-cyan-500/30 glass-panel text-sm leading-relaxed" placeholder="Operational logs, critical events, or fleet discrepancies..."></textarea>
            </section>

            <!-- Execution -->
            <div class="pt-10">
                <button onclick="generateReport()" class="w-full btn-futuristic text-white font-black py-6 px-4 rounded-3xl text-2xl shadow-[0_0_30px_rgba(0,82,255,0.3)] border border-white/10">
                    Generate Mission Report
                </button>
            </div>

            <!-- Output -->
            <div id="outputSection" class="hidden animate-in fade-in slide-in-from-bottom-8 duration-700">
                <div class="glass-panel p-10 rounded-[3rem] border border-cyan-500/20">
                    <h3 class="text-xl font-black mb-6 text-cyan-400 tracking-[0.4em] uppercase italic text-center">Transmission Data</h3>
                    <textarea id="reportOutput" rows="20" class="w-full p-6 rounded-2xl font-mono text-sm leading-relaxed glass-panel mb-8 border-none ring-1 ring-white/10" readonly></textarea>
                    <button onclick="copyToClipboard()" class="w-full bg-cyan-500/10 hover:bg-cyan-500/20 text-cyan-400 font-black py-5 rounded-2xl transition-all border border-cyan-400/30 tracking-widest uppercase">
                        Copy Transmission
                    </button>
                    <div id="copyStatus" class="mt-4 text-xs font-bold text-center text-cyan-400 hidden animate-pulse uppercase tracking-widest">Encrypted content stored in clipboard.</div>
                </div>
            </div>
        </div>
    </div>

    <script>
        const personnelData = {
            missionMDC: [],
            missionMapping: [],
            missionABD: [],
            scouting: [],
            depot: [],
            trainers: [],
            trainees: [],
            buffer: [],
            absent: [],
            ncns: [],
            failedBAC: [],
            late: [], 
            earlyOut: [],
            roadEvents: [],
            rescues: []
        };

        const MAX_ENTRIES = 10;

        function toggleOtherLocation() {
            const locSelect = document.getElementById('location');
            const otherInput = document.getElementById('otherLocation');
            if (locSelect.value === 'Other') {
                otherInput.classList.remove('hidden');
                otherInput.focus();
            } else {
                otherInput.classList.add('hidden');
                otherInput.value = '';
            }
        }

        function autoSelectShift() {
            const shifts = [
                { value: "12:00 AM - 9:00 AM", startHour: 0 },
                { value: "4:00 AM - 1:00 PM", startHour: 4 },
                { value: "8:00 AM - 5:00 PM", startHour: 8 },
                { value: "12:00 PM - 9:00 PM", startHour: 12 },
                { value: "4:00 PM - 1:00 AM", startHour: 16 },
                { value: "8:00 PM - 5:00 AM", startHour: 20 }
            ];
            const currentHour = new Date().getHours();
            let selectedShiftValue = shifts[0].value; 
            for (let i = shifts.length - 1; i >= 0; i--) {
                if (currentHour >= shifts[i].startHour) {
                    selectedShiftValue = shifts[i].value;
                    break;
                }
            }
            document.getElementById('shift').value = selectedShiftValue;
        }

        document.addEventListener('DOMContentLoaded', () => {
            const now = new Date();
            const dateStr = now.toLocaleDateString('en-US', { year: 'numeric', month: '2-digit', day: '2-digit' });
            const timeStr = now.toLocaleTimeString('en-US', { hour: '2-digit', minute: '2-digit', hour12: true });
            document.getElementById('timestamp').textContent = `${dateStr} • ${timeStr} • SYSTEM ACTIVE`;

            autoSelectShift();
            Object.keys(personnelData).forEach(key => renderList(key));
            ['requiredHC', 'plannedHC', 'actualHC'].forEach(id => {
                document.getElementById(id).addEventListener('input', updateSummary);
            });
        });

        function updateSummary() {
            const actualHC = parseInt(document.getElementById('actualHC').value) || 0;
            const headcountWarning = document.getElementById('headcountWarning');

            const presentCount = personnelData.missionMDC.length +
                                 personnelData.missionMapping.length +
                                 personnelData.missionABD.length +
                                 personnelData.scouting.length +
                                 personnelData.depot.length +
                                 personnelData.trainers.length +
                                 personnelData.trainees.length +
                                 personnelData.buffer.length;

            const attendedWithExceptionCount = personnelData.late.length +
                                               personnelData.earlyOut.length +
                                               personnelData.roadEvents.length +
                                               personnelData.rescues.length;
            
            const totalAttending = presentCount + attendedWithExceptionCount;
            const hardAbsenceCount = personnelData.absent.length + personnelData.ncns.length + personnelData.failedBAC.length;

            const totalPersonnelAccountedFor = totalAttending + hardAbsenceCount;
            
            if (actualHC !== totalPersonnelAccountedFor && actualHC > 0) {
                headcountWarning.textContent = `CRITICAL ALERT: MANUAL HEADCOUNT (${actualHC}) MISMATCH WITH SYSTEM ROSTER (${totalPersonnelAccountedFor}).`;
                headcountWarning.classList.remove('hidden');
            } else {
                headcountWarning.classList.add('hidden');
            }

            document.getElementById('totalAttending').textContent = totalAttending;
            document.getElementById('totalAbsences').textContent = hardAbsenceCount;
        }

        function removeInput(section, index) {
            personnelData[section].splice(index, 1);
            renderList(section);
            updateSummary();
        }

        function updatePersonnelData(section, index, type, value) {
            if (type === 'text') personnelData[section][index] = value;
            else if (type === 'name') {
                if (!personnelData[section][index]) personnelData[section][index] = { name: '', time: '' };
                personnelData[section][index].name = value;
            } else if (type === 'time') {
                if (!personnelData[section][index]) personnelData[section][index] = { name: '', time: '' };
                personnelData[section][index].time = value;
            }
        }

        function renderList(section) {
            const container = document.getElementById(section + '_inputs');
            if (!container) return;
            container.innerHTML = '';

            personnelData[section].forEach((item, index) => {
                const isTextTime = ['late', 'earlyOut', 'rescues'].includes(section);
                const wrapper = document.createElement('div');
                wrapper.className = 'flex gap-2 items-center group animate-in slide-in-from-left-2 duration-200';

                if (!isTextTime) {
                    const input = document.createElement('input');
                    input.type = 'text';
                    input.placeholder = (section === 'roadEvents') ? 'Ticket ID' : 'Full Name';
                    input.value = item || '';
                    input.className = 'flex-grow p-3 rounded-xl text-xs input-glow';
                    input.oninput = (e) => updatePersonnelData(section, index, 'text', e.target.value);
                    wrapper.appendChild(input);
                } else {
                    const nameInput = document.createElement('input');
                    nameInput.type = 'text';
                    nameInput.placeholder = (section === 'rescues') ? 'Rescue Unit' : 'Full Name';
                    nameInput.value = item.name || '';
                    nameInput.className = 'flex-grow p-3 rounded-xl text-xs input-glow';
                    nameInput.oninput = (e) => updatePersonnelData(section, index, 'name', e.target.value);
                    
                    const timeInput = document.createElement('input');
                    timeInput.type = 'time';
                    timeInput.value = item.time || '';
                    timeInput.className = 'w-28 p-3 rounded-xl text-xs input-glow';
                    timeInput.oninput = (e) => updatePersonnelData(section, index, 'time', e.target.value);
                    
                    wrapper.appendChild(nameInput);
                    wrapper.appendChild(timeInput);
                }

                const removeBtn = document.createElement('button');
                removeBtn.innerHTML = '✕';
                removeBtn.className = 'text-red-500/30 hover:text-red-500 font-bold px-3 py-1 transition-colors';
                removeBtn.onclick = () => removeInput(section, index);
                wrapper.appendChild(removeBtn);
                container.appendChild(wrapper);
            });
        }

        window.addInput = (section, type) => {
            if (personnelData[section].length < MAX_ENTRIES) {
                personnelData[section].push(type === 'text-time' ? { name: '', time: '' } : '');
                renderList(section);
                updateSummary();
            }
        };

        window.generateReport = () => {
            const locSelect = document.getElementById('location').value;
            const otherLoc = document.getElementById('otherLocation').value;
            const finalLocation = (locSelect === 'Other') ? (otherLoc || 'Unknown Site') : locSelect;
            
            const shift = document.getElementById('shift').value;
            const timestamp = document.getElementById('timestamp').textContent.split(' • ')[0];
            const requiredHC = document.getElementById('requiredHC').value;
            const plannedHC = document.getElementById('plannedHC').value;
            const actualHC = document.getElementById('actualHC').value;
            const firstCar = document.getElementById('firstCar').value;
            const lastCar = document.getElementById('lastCar').value;
            const totalAVs = document.getElementById('totalAVs').value;
            const downAVs = document.getElementById('downAVs').value;
            const availableAVs = document.getElementById('availableAVs').value;
            const trainingAVs = document.getElementById('trainingAVs').value;
            const totalAttending = document.getElementById('totalAttending').textContent;
            const totalAbsences = document.getElementById('totalAbsences').textContent;
            const notes = document.getElementById('notes').value.trim();

            const formatList = (section, formatter = (item) => item) => {
                const list = personnelData[section].map(formatter).filter(s => (s.trim() !== '' && s.trim() !== '@'));
                return list.length > 0 ? '\n' + list.join('\n') : ' N/A';
            };
            const nameTimeFormatter = (item) => `${item.name || '[Missing]'} @ ${item.time || '[Missing]'}`;
            const textFormatter = (item) => item;

            const reportText = `
${finalLocation.toUpperCase()} WAYMO DEPOT REPORT
${timestamp}
Shift: ${shift}

HEAD COUNT SUMMARY
-----------------------------------
Required Head Count: ${requiredHC}
Planned Head Count: ${plannedHC}
Actual Head Count (Manual): ${actualHC}
Total Attending (Calculated): ${totalAttending}
Total Absences (Calculated): ${totalAbsences}

PERSONNEL BREAKDOWN (PRESENT)
-----------------------------------
Mission: MDC:${formatList('missionMDC', textFormatter)}

Mission: Mapping:${formatList('missionMapping', textFormatter)}

Mission: ABD:${formatList('missionABD', textFormatter)}

Mission: Scouting:${formatList('scouting', textFormatter)}

Depot:${formatList('depot', textFormatter)}

Trainers:${formatList('trainers', textFormatter)}

Trainees:${formatList('trainees', textFormatter)}

Buffer:${formatList('buffer', textFormatter)}

ABSENCES AND EXCEPTIONS
-----------------------------------
Absent:${formatList('absent', textFormatter)}

No Call No Show:${formatList('ncns', textFormatter)}

Failed Alert Meter/BAC Exam:${formatList('failedBAC', textFormatter)}

Late w/ Time of Arrival:${formatList('late', nameTimeFormatter)}

Early Out w/ Time of Departure:${formatList('earlyOut', nameTimeFormatter)}

Road Events w/ Ticket ID:${formatList('roadEvents', textFormatter)}

Rescues w/ Time of Return:${formatList('rescues', nameTimeFormatter)}

VEHICLE METRICS
-----------------------------------
First car out: ${firstCar || 'N/A'}
Last car out: ${lastCar || 'N/A'}

Total AV's: ${totalAVs}
Total AV's down: ${downAVs}
Total AV's available: ${availableAVs}
Total AV's in training: ${trainingAVs}

NOTES / ADDITIONAL SHIFT DETAILS
-----------------------------------
${notes || 'N/A'}
`.trim();

            document.getElementById('reportOutput').value = reportText;
            document.getElementById('outputSection').classList.remove('hidden');
            document.getElementById('outputSection').scrollIntoView({ behavior: 'smooth' });
        };

        window.copyToClipboard = () => {
            const copyText = document.getElementById("reportOutput");
            copyText.select();
            document.execCommand('copy');
            const status = document.getElementById('copyStatus');
            status.classList.remove('hidden');
            setTimeout(() => status.classList.add('hidden'), 2000);
        };
    </script>
</body>
</html>
