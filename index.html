import React, { useState, useEffect, useCallback, useMemo } from 'react';
import { Aperture, Search, BookOpen, Layers, CheckCircle, Sun, Moon, Zap, User, MessageSquare, Send, Heart, X, MinusCircle, PlusCircle, Globe, Edit, FileText, Download, TrendingUp, Cpu, Maximize, Menu, RefreshCw, Lightbulb, Filter, ArrowLeft, GitBranch, Terminal } from 'lucide-react';

// --- CONFIGURATION AND SETUP ---
const GEMINI_MODEL = "gemini-2.5-flash-preview-05-20";
const API_KEY = ""; // Placeholder for Gemini API Key. Will be provided by the environment.
const GEMINI_API_URL = `https://generativelanguage.googleapis.com/v1beta/models/${GEMINI_MODEL}:generateContent?key=${API_KEY}`;
const MAX_RETRIES = 3;

// --- CONSTANTS AND DATA ---

const STEPS = [
    { id: 1, title: 'Define Research Question (PICO)', icon: Aperture, description: 'Establish the focus using the PICO framework (Population, Intervention, Comparison, Outcome).' },
    { id: 2, title: 'Protocol & Eligibility Criteria', icon: BookOpen, description: 'Document your methods and selection criteria to ensure rigor and replicability.' },
    { id: 3, title: 'Search Strategy Development', icon: Globe, description: 'Formulate comprehensive search strings for all relevant databases.' },
    { id: 4, title: 'Literature Search & Library', icon: Search, description: 'Execute searches in databases like PubMed. Save and manage results.' },
    { id: 5, title: 'Screening (PRISMA Flow)', icon: Layers, description: 'Systematically screen titles/abstracts, then full-texts, documenting exclusions.' },
    { id: 6, title: 'Data Extraction', icon: Edit, description: 'Extract relevant data points from included studies into a standardized format.' },
    { id: 7, title: 'Risk of Bias Assessment', icon: MinusCircle, description: 'Evaluate the methodological quality and potential biases in included studies.' },
    { id: 8, title: 'Data Synthesis', icon: TrendingUp, description: 'Perform qualitative synthesis or meta-analysis to combine findings.' },
    { id: 9, title: 'Report Writing (PRISMA)', icon: FileText, description: 'Write the final report using guidelines like the PRISMA checklist.' },
];

const JOURNAL_TYPES = [
    { value: 'all', label: 'All Journal Types' },
    { value: 'rct', label: 'RCT/Trials Focused' },
    { value: 'review', label: 'Reviews/Meta-Analyses' },
    { value: 'diagnostic', label: 'Diagnostic Accuracy' },
    { value: 'general', label: 'General Medicine' },
];

const initialChatHistory = [
    { role: 'assistant', text: "Hello! I'm your Research Buddy. I can help you with systematic review questions, PICO, search strategies, and more. What can I do for you today?" },
];

const initialProjectData = {
    pico: { population: 'e.g., Adults with knee osteoarthritis', intervention: 'e.g., Tai Chi exercise', comparison: 'e.g., Standard physiotherapy', outcome: 'e.g., Pain severity scores (VAS)' },
    protocol: 'Document all planned methods here...',
    eligibilityCriteria: 'Inclusion: English language, randomized controlled trials (RCTs). Exclusion: Cohort studies, non-human trials.',
    searchStrategy: 'Example: (("Tai Chi") OR ("Qigong")) AND (("Knee Osteoarthritis") OR ("OA")) AND ("RCT" [Publication Type])',
    prisma: { // Mock data for PRISMA flow chart
        identified: 0,
        screened: 0,
        fullTextAssessed: 0,
        included: 0,
    },
    library: [], // Saved papers
};

// --- UTILITY FUNCTIONS ---

/**
 * Executes a fetch request with exponential backoff for resilience.
 */
const safeFetch = async (url, options, retries = 0) => {
    try {
        const response = await fetch(url, options);
        if (!response.ok) {
            throw new Error(`HTTP error! status: ${response.status}`);
        }
        return response.json();
    } catch (error) {
        if (retries < MAX_RETRIES) {
            const delay = Math.pow(2, retries) * 1000;
            await new Promise(resolve => setTimeout(resolve, delay));
            return safeFetch(url, options, retries + 1);
        } else {
            throw new Error("API call failed after max retries.");
        }
    }
};

/**
 * Executes a Gemini API call with a system prompt and query.
 */
const callGemini = async (userQuery, systemPrompt, tools = null) => {
    const payload = {
        contents: [{ parts: [{ text: userQuery }] }],
        systemInstruction: { parts: [{ text: systemPrompt }] },
    };
    if (tools) {
        payload.tools = tools;
    }

    try {
        const response = await safeFetch(GEMINI_API_URL, {
            method: 'POST',
            headers: { 'Content-Type': 'application/json' },
            body: JSON.stringify(payload)
        });

        return response.candidates?.[0]?.content?.parts?.[0]?.text || "Sorry, I couldn't process that request.";

    } catch (error) {
        console.error("Gemini API Error:", error);
        return "I apologize, but I encountered an error during the AI process. Please try again later.";
    }
};

/**
 * Mock function to simulate combined PubMed and Web search results.
 */
const fetchLiteratureResults = async (query, journalType) => {
    // Simulated PubMed/Web data based on query and filter
    const allMockData = [
        {
            pmid: '37895200',
            title: 'Effect of virtual reality training on surgical skill acquisition: a systematic review and meta-analysis.',
            authors: 'Smith J, Doe A, Chen L.',
            year: 2023,
            abstract: 'We conducted a systematic review to evaluate the effectiveness of virtual reality (VR) training compared to traditional methods for improving surgical skills...',
            source: 'PubMed',
            journalType: 'review',
            link: 'https://pubmed.ncbi.nlm.nih.gov/37895200/',
            isSaved: false,
        },
        {
            pmid: '37895201',
            title: 'Long-term outcomes of robotic-assisted surgery for prostate cancer: a PRISMA-compliant RCT.',
            authors: 'Wang P, Garcia M.',
            year: 2022,
            abstract: 'This randomized controlled trial synthesized evidence on the long-term oncological and functional outcomes following robotic-assisted radical prostatectomy...',
            source: 'PubMed',
            journalType: 'rct',
            link: 'https://pubmed.ncbi.nlm.nih.gov/37895201/',
            isSaved: false,
        },
        {
            pmid: 'WEB-4001',
            title: 'Emerging trends in AI for diagnostic accuracy in dermatology.',
            authors: 'Kaur S, Patel R, Johnson K.',
            year: 2024,
            abstract: 'AI applications in medical imaging for cancer detection are rapidly evolving. We assessed the diagnostic accuracy and clinical utility of these systems in a hospital setting.',
            source: 'Web Grounding (Journal of Dermatology)',
            journalType: 'diagnostic',
            link: 'https://www.example.com/ai-dermatology',
            isSaved: false,
        },
        {
            pmid: '37895203',
            title: 'General principles of clinical trial design for new interventions.',
            authors: 'Baker T, Miller S.',
            year: 2021,
            abstract: 'An overview of the essential design elements necessary for high-quality clinical trials in a general medical context.',
            source: 'PubMed',
            journalType: 'general',
            link: 'https://pubmed.ncbi.nlm.nih.gov/37895203/',
            isSaved: false,
        },
    ];

    await new Promise(resolve => setTimeout(resolve, 1500)); // Simulate network delay

    const filteredData = allMockData
        .filter(p => p.title.toLowerCase().includes(query.toLowerCase()) || query.toLowerCase().includes('vr'))
        .filter(p => journalType === 'all' || p.journalType === journalType);

    return {
        query,
        count: filteredData.length,
        papers: filteredData,
    };
};


// --- REUSABLE COMPONENTS ---

/**
 * Theme Toggle Component
 */
const ThemeToggle = ({ theme, setTheme }) => {
    const isDark = theme === 'dark';
    const toggleTheme = () => {
        const newTheme = isDark ? 'light' : 'dark';
        setTheme(newTheme);
        localStorage.setItem('theme', newTheme);
    };

    return (
        <button
            onClick={toggleTheme}
            className="p-3 rounded-full text-gray-700 dark:text-gray-200 bg-gray-100 dark:bg-gray-700 hover:bg-gray-200 dark:hover:bg-gray-600 transition duration-300 shadow-lg fixed top-4 right-4 z-50 md:top-auto md:right-6 md:bottom-28"
            aria-label="Toggle theme"
        >
            {isDark ? <Moon size={24} /> : <Sun size={24} />}
        </button>
    );
};

/**
 * Floating AI Chatbot Assistant Component (Minimized)
 */
const ChatBotAssistant = ({ onChatStart }) => {
    return (
        <div className="fixed bottom-6 right-6 z-50">
            <button
                onClick={onChatStart}
                className="w-16 h-16 rounded-full bg-indigo-600 hover:bg-indigo-700 text-white shadow-xl flex items-center justify-center transition duration-300 transform hover:scale-105"
                aria-label="Start Research Buddy Chat"
            >
                <Cpu size={32} />
            </button>
        </div>
    );
};

/**
 * Chat Modal Component (Simplified for brevity)
 */
const ChatModal = ({ isVisible, onClose, theme }) => {
    const [history, setHistory] = useState(initialChatHistory);
    const [input, setInput] = useState('');
    const [isLoading, setIsLoading] = useState(false);

    const handleSendMessage = useCallback(async () => {
        if (!input.trim() || isLoading) return;

        const userQuery = input.trim();
        const userMessage = { role: 'user', text: userQuery };
        const newHistory = [...history, userMessage];
        setHistory(newHistory);
        setInput('');
        setIsLoading(true);

        const systemPrompt = "You are 'Research Buddy', an expert assistant for systematic review methodology. Your tone is supportive, clear, and professional. You must answer questions about PICO, search strategies, risk of bias, meta-analysis, and PRISMA. Keep your responses concise and focused on academic rigor.";

        const fullQuery = newHistory.map(msg => `${msg.role === 'user' ? 'User' : 'Assistant'}: ${msg.text}`).join('\n') + `\nUser: ${userQuery}`;

        try {
            const generatedText = await callGemini(fullQuery, systemPrompt, [{ "google_search": {} }]);
            setHistory(prev => [...prev, { role: 'assistant', text: generatedText }]);
        } catch (error) {
            setHistory(prev => [...prev, { role: 'assistant', text: "I apologize, but I encountered an error. Please try again later." }]);
        } finally {
            setIsLoading(false);
        }
    }, [input, history, isLoading]);

    useEffect(() => {
        if (isVisible) {
            const chatBox = document.getElementById('chat-history');
            if (chatBox) chatBox.scrollTop = chatBox.scrollHeight;
        }
    }, [history, isVisible]);

    if (!isVisible) return null;
    const isDark = theme === 'dark';

    return (
        <div className={`fixed bottom-24 right-6 w-11/12 max-w-sm h-3/4 max-h-[600px] ${isDark ? 'bg-gray-800 border-gray-700 text-white' : 'bg-white border-gray-200 text-gray-800'} border rounded-xl shadow-2xl flex flex-col z-50 transition-all duration-300 ease-in-out`}>
            {/* Header and Input (omitted for brevity, assume standard chat modal layout) */}
            <div className={`flex justify-between items-center p-4 rounded-t-xl ${isDark ? 'bg-indigo-700' : 'bg-indigo-600'} text-white`}>
                <h3 className="text-lg font-bold flex items-center"><Cpu size={20} className="mr-2" /> Research Buddy</h3>
                <button onClick={onClose} className="p-1 rounded-full hover:bg-indigo-500 transition" aria-label="Close Chat"><X size={20} /></button>
            </div>
            <div id="chat-history" className="flex-1 overflow-y-auto p-4 space-y-4">
                 {history.map((msg, index) => (
                    <div key={index} className={`flex ${msg.role === 'user' ? 'justify-end' : 'justify-start'}`}>
                        <div className={`max-w-[80%] p-3 rounded-xl shadow-md ${msg.role === 'user'
                                ? 'bg-indigo-100 dark:bg-indigo-900 text-gray-900 dark:text-white rounded-br-none'
                                : 'bg-gray-200 dark:bg-gray-700 text-gray-900 dark:text-white rounded-tl-none'
                            }`}>
                            <p className="font-semibold text-sm mb-1">{msg.role === 'user' ? 'You' : 'Buddy'}</p>
                            <p className="text-sm whitespace-pre-wrap">{msg.text}</p>
                        </div>
                    </div>
                ))}
                {isLoading && (
                    <div className="flex justify-start">
                        <div className="max-w-[80%] p-3 rounded-xl bg-gray-200 dark:bg-gray-700 rounded-tl-none">
                            <div className="flex space-x-1">
                                <div className="h-2 w-2 bg-indigo-600 rounded-full animate-bounce [animation-delay:-0.3s]"></div>
                                <div className="h-2 w-2 bg-indigo-600 rounded-full animate-bounce [animation-delay:-0.15s]"></div>
                                <div className="h-2 w-2 bg-indigo-600 rounded-full animate-bounce"></div>
                            </div>
                        </div>
                    </div>
                )}
            </div>
            <div className="p-4 border-t dark:border-gray-700">
                <div className="flex items-center">
                    <input
                        type="text"
                        value={input}
                        onChange={(e) => setInput(e.target.value)}
                        onKeyPress={(e) => e.key === 'Enter' && handleSendMessage()}
                        placeholder="Ask your research question..."
                        className={`flex-1 p-3 border rounded-l-lg focus:outline-none focus:ring-2 focus:ring-indigo-500 ${isDark ? 'bg-gray-700 border-gray-600 text-white' : 'bg-gray-50 border-gray-300 text-gray-900'}`}
                        disabled={isLoading}
                    />
                    <button
                        onClick={handleSendMessage}
                        className={`p-3 rounded-r-lg ${isLoading ? 'bg-gray-400' : 'bg-indigo-600 hover:bg-indigo-700'} text-white transition duration-300`}
                        disabled={isLoading}
                    >
                        <Send size={20} />
                    </button>
                </div>
            </div>
        </div>
    );
};

// --- NEW FEATURES & COMPONENTS ---

/**
 * AI Topic Generator Component for the Landing Page
 */
const TopicGenerator = () => {
    const [field, setField] = useState('');
    const [topics, setTopics] = useState('');
    const [isLoading, setIsLoading] = useState(false);

    const generateTopics = async () => {
        if (!field.trim()) return;

        setIsLoading(true);
        setTopics('');

        const systemPrompt = "You are a creative academic research topic generator. Based on the user's field of interest, suggest 3 highly specific, novel, and researchable systematic review or meta-analysis topics. Format the output clearly as a numbered list with titles and brief (one-sentence) rationales for novelty.";
        const userQuery = `Generate 3 systematic review topics for the field of: ${field}`;

        const result = await callGemini(userQuery, systemPrompt);
        setTopics(result);
        setIsLoading(false);
    };

    return (
        <div className="p-6 bg-white dark:bg-gray-800 rounded-xl shadow-lg border border-gray-100 dark:border-gray-700">
            <h3 className="text-xl font-bold text-indigo-600 dark:text-indigo-400 flex items-center mb-4">
                <Lightbulb size={24} className="mr-2" /> AI Topic Generator
            </h3>
            <p className="text-gray-600 dark:text-gray-400 mb-4">
                Enter your broad area of interest (e.g., 'Telemedicine in chronic care' or 'Pediatric oncology') to generate 3 focused systematic review ideas.
            </p>
            <div className="flex space-x-2 mb-4">
                <input
                    type="text"
                    value={field}
                    onChange={(e) => setField(e.target.value)}
                    placeholder="Enter research field..."
                    className="flex-1 p-3 border rounded-lg dark:bg-gray-700 dark:border-gray-600 focus:ring-indigo-500 focus:border-indigo-500"
                />
                <button
                    onClick={generateTopics}
                    disabled={isLoading || !field.trim()}
                    className="px-4 py-3 bg-indigo-600 text-white rounded-lg hover:bg-indigo-700 disabled:bg-indigo-400 transition flex items-center"
                >
                    {isLoading ? <RefreshCw size={20} className="animate-spin mr-2" /> : <Zap size={20} className="mr-2" />}
                    Generate
                </button>
            </div>

            {topics && (
                <div className="mt-6 p-4 bg-indigo-50 dark:bg-indigo-900/50 rounded-lg whitespace-pre-wrap text-sm text-indigo-900 dark:text-indigo-100">
                    <h4 className="font-bold mb-2">Suggested Topics:</h4>
                    {topics}
                </div>
            )}
        </div>
    );
};

/**
 * Landing Page Component
 */
const LandingPage = ({ startReview }) => (
    <div className="p-4 md:p-12 max-w-4xl mx-auto min-h-[80vh] flex flex-col justify-center">
        <header className="text-center mb-12">
            <h1 className="text-5xl md:text-6xl font-extrabold text-gray-900 dark:text-white leading-tight">
                <span className="text-indigo-600 dark:text-indigo-400">MOD MED</span> MAP
            </h1>
            <p className="mt-4 text-xl text-gray-600 dark:text-gray-400">
                Your AI-Powered Assistant for Systematic Reviews, from topic generation to final report.
            </p>
        </header>

        <TopicGenerator />

        <section className="mt-10 text-center">
            <h2 className="text-2xl font-bold text-gray-800 dark:text-gray-200 mb-4">Ready to start your review?</h2>
            <button
                onClick={() => startReview('dashboard')}
                className="px-8 py-4 text-lg font-bold bg-green-600 text-white rounded-xl shadow-lg hover:bg-green-700 transition transform hover:scale-[1.02]"
            >
                <Layers size={24} className="inline mr-2" /> Start 9-Step Review Dashboard
            </button>
        </section>

        <section className="mt-12 p-6 border-t dark:border-gray-700">
            <h3 className="text-xl font-bold text-gray-800 dark:text-gray-200 flex items-center mb-4">
                <Terminal size={24} className="mr-2 text-indigo-500" /> Key Features
            </h3>
            <div className="grid grid-cols-1 md:grid-cols-3 gap-6 text-center">
                <div className="p-4 rounded-lg bg-gray-100 dark:bg-gray-700">
                    <Aperture className="w-8 h-8 mx-auto text-indigo-600 dark:text-indigo-400 mb-2" />
                    <p className="font-semibold">Structured Workflow</p>
                    <p className="text-sm text-gray-600 dark:text-gray-400">9 phases based on PRISMA guidelines.</p>
                </div>
                <div className="p-4 rounded-lg bg-gray-100 dark:bg-gray-700">
                    <Lightbulb className="w-8 h-8 mx-auto text-indigo-600 dark:text-indigo-400 mb-2" />
                    <p className="font-semibold">AI Topic Ideation</p>
                    <p className="text-sm text-gray-600 dark:text-gray-400">Generate novel and specific research questions.</p>
                </div>
                <div className="p-4 rounded-lg bg-gray-100 dark:bg-gray-700">
                    <GitBranch className="w-8 h-8 mx-auto text-indigo-600 dark:text-indigo-400 mb-2" />
                    <p className="font-semibold">Gap Analysis</p>
                    <p className="text-sm text-gray-600 dark:text-gray-400">Automatically find gaps in search results.</p>
                </div>
            </div>
        </section>

    </div>
);

/**
 * PubMed Paper Card Component (from previous version, updated for source)
 */
const PaperCard = ({ paper, onToggleSave }) => {
    const isSaved = paper.isSaved;
    const isWeb = paper.source.includes('Web');

    return (
        <div className="p-4 border-b dark:border-gray-700 hover:bg-gray-50 dark:hover:bg-gray-700 transition duration-150">
            <div className="flex justify-between items-start">
                <h4 className="font-bold text-base leading-snug">{paper.title}</h4>
                <button
                    onClick={() => onToggleSave(paper.pmid)}
                    className={`ml-4 p-2 rounded-full transition duration-200 ${isSaved ? 'text-red-500 bg-red-100 dark:bg-red-900' : 'text-gray-400 hover:text-indigo-600 dark:hover:text-indigo-400 hover:bg-gray-100 dark:hover:bg-gray-600'}`}
                    aria-label={isSaved ? "Remove from Library" : "Save to Library"}
                >
                    <Heart size={20} fill={isSaved ? 'currentColor' : 'none'} />
                </button>
            </div>
            <div className="text-sm text-gray-600 dark:text-gray-400 mt-1">
                {paper.authors} ({paper.year}) <span className={`inline-flex items-center ml-2 px-2 py-0.5 rounded-full text-xs font-medium ${isWeb ? 'bg-green-100 text-green-800 dark:bg-green-900 dark:text-green-200' : 'bg-blue-100 text-blue-800 dark:bg-blue-900 dark:text-blue-200'}`}>
                    <Globe size={10} className="mr-1" /> {paper.source}
                </span>
            </div>
            <p className="text-sm mt-2 line-clamp-2">{paper.abstract}</p>
            <div className="mt-2 flex space-x-3 text-sm">
                <a href={paper.link} target="_blank" rel="noopener noreferrer" className="text-indigo-600 dark:text-indigo-400 hover:underline flex items-center">
                    <BookOpen size={14} className="mr-1" /> View Source
                </a>
            </div>
        </div>
    );
};

/**
 * Enhanced Literature Search (Step 4)
 */
const EnhancedLiteratureSearch = ({ library, setLibrary }) => {
    const [searchQuery, setSearchQuery] = useState('VR surgery');
    const [journalType, setJournalType] = useState('all');
    const [searchResults, setSearchResults] = useState(null);
    const [isLoading, setIsLoading] = useState(false);
    const [viewMode, setViewMode] = useState('search'); // 'search' or 'library'
    const [gapAnalysis, setGapAnalysis] = useState('');
    const [isGapLoading, setIsGapLoading] = useState(false);

    const handleSearch = async (e) => {
        e.preventDefault();
        if (!searchQuery.trim()) return;

        setIsLoading(true);
        setSearchResults(null);
        setGapAnalysis(''); // Clear previous gap analysis

        try {
            const data = await fetchLiteratureResults(searchQuery, journalType);

            // Merge 'isSaved' status from library into new results
            const updatedPapers = data.papers.map(p => ({
                ...p,
                isSaved: library.some(libP => libP.pmid === p.pmid)
            }));
            setSearchResults({ ...data, papers: updatedPapers });
        } catch (error) {
            setSearchResults({ count: 0, papers: [], error: 'Failed to fetch results. Check console for details.' });
        } finally {
            setIsLoading(false);
        }
    };

    const handleFindGaps = async () => {
        if (!searchResults || searchResults.papers.length === 0) {
            setGapAnalysis('Please perform a search with results first.');
            return;
        }

        setIsGapLoading(true);
        setGapAnalysis('');

        const paperSummary = searchResults.papers.map(p => `- ${p.title} (Abstract: ${p.abstract.substring(0, 100)}...)`).join('\n');
        const userQuery = `Analyze these search results and identify research gaps:\n\n${paperSummary}`;
        const systemPrompt = "Analyze the provided list of research paper titles and abstracts. Identify 2-3 specific, relevant, and contemporary research gaps or limitations that future systematic reviews could address. Focus on missing populations, interventions, comparisons, or long-term data. Present the gaps clearly with a title and a brief explanation.";

        const result = await callGemini(userQuery, systemPrompt);
        setGapAnalysis(result);
        setIsGapLoading(false);
    };

    const handleToggleSave = useCallback((pmid) => {
        setLibrary(prevLibrary => {
            const paperIndex = prevLibrary.findIndex(p => p.pmid === pmid);
            if (paperIndex > -1) {
                return prevLibrary.filter(p => p.pmid !== pmid);
            } else {
                const paperToAdd = searchResults?.papers.find(p => p.pmid === pmid) || library.find(p => p.pmid === pmid);
                if (paperToAdd) {
                    return [...prevLibrary, { ...paperToAdd, isSaved: true }];
                }
                // Handle case where paper might be added from an external source (mock)
                return [...prevLibrary, { pmid, title: `Manual Add: ${pmid}`, authors: 'N/A', year: new Date().getFullYear(), abstract: 'Manual entry.', source: 'Manual', link: '#', isSaved: true }];
            }
        });
        // Update search results to reflect save state
        if (searchResults) {
             setSearchResults(prevResults => ({
                ...prevResults,
                papers: prevResults.papers.map(p => p.pmid === pmid ? { ...p, isSaved: !p.isSaved } : p)
            }));
        }
    }, [library, searchResults]);

    const displayPapers = viewMode === 'search' ? searchResults?.papers : library;
    const paperCount = viewMode === 'search' ? searchResults?.count : library.length;

    return (
        <div className="mt-4 p-4 border rounded-xl dark:border-gray-700 bg-white dark:bg-gray-800 shadow-inner">
            <div className="flex mb-4 border-b dark:border-gray-700">
                <button
                    className={`flex-1 p-3 font-semibold transition-colors ${viewMode === 'search' ? 'text-indigo-600 dark:text-indigo-400 border-b-2 border-indigo-600 dark:border-indigo-400' : 'text-gray-500 dark:text-gray-400 hover:text-indigo-600'}`}
                    onClick={() => setViewMode('search')}
                >
                    <Search size={18} className="inline mr-2" /> Search Papers
                </button>
                <button
                    className={`flex-1 p-3 font-semibold transition-colors ${viewMode === 'library' ? 'text-indigo-600 dark:text-indigo-400 border-b-2 border-indigo-600 dark:border-indigo-400' : 'text-gray-500 dark:text-gray-400 hover:text-indigo-600'}`}
                    onClick={() => setViewMode('library')}
                >
                    <BookOpen size={18} className="inline mr-2" /> Project Library ({library.length})
                </button>
            </div>

            {viewMode === 'search' && (
                <form onSubmit={handleSearch} className="space-y-4">
                    <div className="flex flex-col md:flex-row md:space-x-2">
                        <input
                            type="text"
                            value={searchQuery}
                            onChange={(e) => setSearchQuery(e.target.value)}
                            placeholder="Enter PICO terms or search query for PubMed/Web"
                            className="flex-1 p-3 border rounded-lg dark:bg-gray-700 dark:border-gray-600 focus:ring-indigo-500 focus:border-indigo-500 mb-2 md:mb-0"
                            required
                        />
                        <select
                            value={journalType}
                            onChange={(e) => setJournalType(e.target.value)}
                            className="w-full md:w-auto p-3 border rounded-lg dark:bg-gray-700 dark:border-gray-600 focus:ring-indigo-500 focus:border-indigo-500"
                        >
                            {JOURNAL_TYPES.map(type => (
                                <option key={type.value} value={type.value}>{type.label}</option>
                            ))}
                        </select>
                    </div>
                    <button
                        type="submit"
                        disabled={isLoading}
                        className="w-full px-4 py-3 bg-indigo-600 text-white rounded-lg hover:bg-indigo-700 disabled:bg-indigo-400 transition"
                    >
                        {isLoading ? 'Searching...' : 'Search PubMed & Web'}
                    </button>
                </form>
            )}

            {(viewMode === 'search' || (viewMode === 'library' && library.length > 0)) && (
                <div className="mt-6 space-y-4">
                    <button
                        onClick={handleFindGaps}
                        disabled={isGapLoading || !searchResults || searchResults.papers.length === 0}
                        className="w-full px-4 py-3 bg-yellow-600 text-white rounded-lg hover:bg-yellow-700 disabled:bg-yellow-400 transition flex items-center justify-center"
                    >
                        {isGapLoading ? <RefreshCw size={20} className="animate-spin mr-2" /> : <GitBranch size={20} className="mr-2" />}
                        {isGapLoading ? 'Analyzing Gaps...' : 'AI Find Research Gaps'}
                    </button>

                    {gapAnalysis && (
                        <div className="p-4 bg-yellow-50 dark:bg-yellow-900/50 rounded-lg text-sm text-yellow-900 dark:text-yellow-100 whitespace-pre-wrap">
                            <h4 className="font-bold mb-2 flex items-center"><Lightbulb size={18} className="mr-2" /> Research Gap Analysis:</h4>
                            {gapAnalysis}
                        </div>
                    )}
                </div>
            )}

            <div className="mt-4 min-h-[200px] max-h-[400px] overflow-y-auto scrollbar-thin">
                {isLoading && viewMode === 'search' && (
                    <p className="text-center p-8 text-indigo-500 animate-pulse">Loading search results (simulated delay)...</p>
                )}
                {!isLoading && (
                    <>
                        {paperCount > 0 && (
                            <p className="text-sm text-gray-500 dark:text-gray-400 mb-2">
                                {viewMode === 'search' ? `Found ${searchResults.count} results (Filtered by Journal Type).` : `Showing ${library.length} saved papers.`}
                            </p>
                        )}
                        {(displayPapers?.length > 0) ? (
                            displayPapers.map(paper => (
                                <PaperCard key={paper.pmid} paper={paper} onToggleSave={handleToggleSave} />
                            ))
                        ) : (
                            <p className="text-center p-8 text-gray-500 dark:text-gray-400">
                                {viewMode === 'search'
                                    ? 'Enter a query and hit search to find relevant literature.'
                                    : 'Your Project Library is currently empty. Save papers from the search tab to start building your library.'}
                            </p>
                        )}
                    </>
                )}
            </div>
        </div>
    );
};

// --- DASHBOARD COMPONENTS (Modified from previous version) ---

const DataInput = ({ label, value, onChange }) => (
    <div className="flex flex-col">
        <label className="text-sm font-medium mb-1 text-gray-700 dark:text-gray-300">{label}</label>
        <input
            type="number"
            min="0"
            value={value}
            onChange={onChange}
            className="p-3 border rounded-lg dark:bg-gray-700 dark:border-gray-600 focus:ring-indigo-500 focus:border-indigo-500"
        />
    </div>
);


const StepCard = ({ step, projectData, setProjectData, library, setLibrary }) => {
    const { id, title, description, icon: Icon } = step;

    const handlePicoChange = (field, value) => {
        setProjectData(prev => ({
            ...prev,
            pico: { ...prev.pico, [field]: value }
        }));
    };

    const handleInputChange = (key, value) => {
        setProjectData(prev => ({ ...prev, [key]: value }));
    };

    const handlePrismaChange = (key, value) => {
        const num = parseInt(value) || 0;
        setProjectData(prev => ({
            ...prev,
            prisma: { ...prev.prisma, [key]: num }
        }));
    };

    let content;
    switch (id) {
        case 1: // PICO
            content = (
                <div className="space-y-4">
                    {['population', 'intervention', 'comparison', 'outcome'].map(field => (
                        <div key={field}>
                            <label className="block text-sm font-medium capitalize mb-1 text-gray-700 dark:text-gray-300">{field}</label>
                            <textarea
                                rows="2"
                                value={projectData.pico[field]}
                                onChange={(e) => handlePicoChange(field, e.target.value)}
                                placeholder={initialProjectData.pico[field]}
                                className="w-full p-3 border rounded-lg dark:bg-gray-700 dark:border-gray-600 focus:ring-indigo-500 focus:border-indigo-500"
                            />
                        </div>
                    ))}
                </div>
            );
            break;
        case 2: // Protocol & Eligibility (Content omitted for brevity, logic remains as before)
            content = (
                <div className="space-y-4">
                    <div>
                        <label className="block text-sm font-medium mb-1 text-gray-700 dark:text-gray-300">Protocol Summary</label>
                        <textarea rows="4" value={projectData.protocol} onChange={(e) => handleInputChange('protocol', e.target.value)} placeholder={initialProjectData.protocol} className="w-full p-3 border rounded-lg dark:bg-gray-700 dark:border-gray-600 focus:ring-indigo-500 focus:border-indigo-500" />
                    </div>
                    <div>
                        <label className="block text-sm font-medium mb-1 text-gray-700 dark:text-gray-300">Eligibility Criteria</label>
                        <textarea rows="4" value={projectData.eligibilityCriteria} onChange={(e) => handleInputChange('eligibilityCriteria', e.target.value)} placeholder={initialProjectData.eligibilityCriteria} className="w-full p-3 border rounded-lg dark:bg-gray-700 dark:border-gray-600 focus:ring-indigo-500 focus:border-indigo-500" />
                    </div>
                </div>
            );
            break;
        case 3: // Search Strategy (Content omitted for brevity, logic remains as before)
             content = (
                <div className="space-y-4">
                    <div>
                        <label className="block text-sm font-medium mb-1 text-gray-700 dark:text-gray-300">Full Search Strategy (for PubMed, Embase, etc.)</label>
                        <textarea rows="6" value={projectData.searchStrategy} onChange={(e) => handleInputChange('searchStrategy', e.target.value)} placeholder={initialProjectData.searchStrategy} className="w-full p-3 border rounded-lg font-mono text-sm dark:bg-gray-700 dark:border-gray-600 focus:ring-indigo-500 focus:border-indigo-500" />
                    </div>
                    <div className="p-3 bg-indigo-50 dark:bg-indigo-900/50 rounded-lg text-sm">
                        <p className="font-semibold text-indigo-700 dark:text-indigo-300">Tip:</p>
                        <p className="text-indigo-600 dark:text-indigo-200">Use your PICO terms and Boolean operators (AND, OR, NOT) to build a sensitive and specific search string. Use the Research Buddy for help!</p>
                    </div>
                </div>
            );
            break;
        case 4: // Literature Search (NEW Component)
            content = (
                <EnhancedLiteratureSearch library={library} setLibrary={setLibrary} />
            );
            break;
        case 5: // Screening (Content omitted for brevity, logic remains as before)
            content = (
                <div className="space-y-4">
                    <p className="font-semibold text-lg text-gray-800 dark:text-gray-200">Papers in Library: {library.length}</p>
                    <div className="grid grid-cols-2 gap-4">
                        <DataInput label="Total Identified (Step 4)" value={projectData.prisma.identified} onChange={(e) => handlePrismaChange('identified', e.target.value)} />
                        <DataInput label="Screened (Title/Abstract)" value={projectData.prisma.screened} onChange={(e) => handlePrismaChange('screened', e.target.value)} />
                        <DataInput label="Full-Text Assessed" value={projectData.prisma.fullTextAssessed} onChange={(e) => handlePrismaChange('fullTextAssessed', e.target.value)} />
                        <DataInput label="Included in Synthesis" value={projectData.prisma.included} onChange={(e) => handlePrismaChange('included', e.target.value)} />
                    </div>
                </div>
            );
            break;
        case 6: // Data Extraction (Content omitted for brevity, logic remains as before)
             content = (
                <div className="space-y-4">
                    <p className="p-3 border-l-4 border-yellow-500 bg-yellow-50 dark:bg-yellow-900/30 text-sm text-yellow-800 dark:text-yellow-200">
                        Create a data extraction form to record critical details like methodology, sample size, outcome measures, and key results for each of the **{projectData.prisma.fullTextAssessed || 0}** studies assessed.
                    </p>
                </div>
            );
            break;
        case 7: // Risk of Bias (Content omitted for brevity, logic remains as before)
             content = (
                <div className="space-y-4">
                    <p className="p-3 border-l-4 border-teal-500 bg-teal-50 dark:bg-teal-900/30 text-sm text-teal-800 dark:text-teal-200">
                        Select an appropriate tool based on your study designs (e.g., **Cochrane RoB 2** for RCTs, **ROBINS-I** for non-RCTs).
                    </p>
                </div>
            );
            break;
        case 8: // Data Synthesis (Content omitted for brevity, logic remains as before)
             content = (
                <div className="space-y-4">
                    <h4 className="font-semibold text-lg text-gray-800 dark:text-gray-200">Type of Synthesis:</h4>
                    <div className="grid grid-cols-1 md:grid-cols-2 gap-4">
                        <div className="p-4 border rounded-lg bg-blue-50 dark:bg-blue-900/30">
                            <h5 className="font-bold mb-1 text-blue-700 dark:text-blue-300">Qualitative Synthesis</h5>
                        </div>
                        <div className="p-4 border rounded-lg bg-green-50 dark:bg-green-900/30">
                            <h5 className="font-bold mb-1 text-green-700 dark:text-green-300">Quantitative Synthesis (Meta-Analysis)</h5>
                        </div>
                    </div>
                </div>
            );
            break;
        case 9: // Report Writing (Content omitted for brevity, logic remains as before)
             content = (
                <div className="space-y-4">
                    <p className="p-3 border-l-4 border-red-500 bg-red-50 dark:bg-red-900/30 text-sm text-red-800 dark:text-red-200">
                        The **PRISMA 2020** statement provides an essential 27-item checklist for reporting systematic reviews.
                    </p>
                    <a href="#" onClick={(e) => e.preventDefault()} className="text-lg font-bold text-indigo-600 dark:text-indigo-400 hover:underline flex items-center">
                        <Maximize size={20} className="mr-2" /> View PRISMA 2020 Checklist
                    </a>
                </div>
            );
            break;
        default:
            content = <p>Content for this step is under development.</p>;
    }

    return (
        <div className="p-6 md:p-8 bg-white dark:bg-gray-800 rounded-xl shadow-lg border border-gray-100 dark:border-gray-700">
            <h3 className="text-2xl font-extrabold text-gray-900 dark:text-white mb-3 flex items-center">
                <Icon size={28} className="mr-3 text-indigo-600 dark:text-indigo-400" />
                {title}
            </h3>
            <p className="text-gray-600 dark:text-gray-400 mb-6 border-b pb-4 dark:border-gray-700">{description}</p>
            {content}
        </div>
    );
};


/**
 * Main Application Component (Router)
 */
const App = () => {
    const [theme, setTheme] = useState(() => {
        const storedTheme = localStorage.getItem('theme');
        return storedTheme || (window.matchMedia('(prefers-color-scheme: dark)').matches ? 'dark' : 'light');
    });

    useEffect(() => {
        if (theme === 'dark') {
            document.documentElement.classList.add('dark');
        } else {
            document.documentElement.classList.remove('dark');
        }
    }, [theme]);

    // Use state to simulate routing: 'landing' or 'dashboard'
    const [currentPage, setCurrentPage] = useState('landing');
    const [currentStep, setCurrentStep] = useState(1);
    const [isSidebarOpen, setIsSidebarOpen] = useState(false);
    const [isChatOpen, setIsChatOpen] = useState(false);

    const [projectData, setProjectData] = useState(initialProjectData);
    const setLibrary = useCallback((newLibrary) => {
        setProjectData(prev => ({ ...prev, library: newLibrary(prev.library) }));
    }, []);
    const library = projectData.library;

    const currentStepData = STEPS.find(step => step.id === currentStep) || STEPS[0];

    const handleStepChange = (stepId) => {
        setCurrentStep(stepId);
        setIsSidebarOpen(false); // Close sidebar on mobile after selection
    };

    // Sidebar/Menu Component
    const Sidebar = () => (
        <nav className="p-4 md:p-6 space-y-2 flex-shrink-0 w-full md:w-64 bg-white dark:bg-gray-800 shadow-xl md:shadow-none border-b md:border-r dark:border-gray-700 md:h-full md:sticky md:top-0 md:pt-24">
            <button
                onClick={() => setCurrentPage('landing')}
                className="w-full text-left p-3 rounded-xl transition duration-200 flex items-center text-red-600 dark:text-red-400 hover:bg-red-50 dark:hover:bg-red-900 mb-4 font-bold"
            >
                <ArrowLeft size={20} className="mr-3" /> Back to Tools
            </button>
            <h3 className="text-lg font-bold text-gray-900 dark:text-white mb-4 hidden md:block">Research Phases</h3>
            {STEPS.map((step) => {
                const isActive = step.id === currentStep;
                return (
                    <button
                        key={step.id}
                        onClick={() => handleStepChange(step.id)}
                        className={`w-full text-left p-3 rounded-xl transition duration-200 flex items-center group
                            ${isActive
                                ? 'bg-indigo-600 text-white shadow-lg'
                                : 'text-gray-700 dark:text-gray-300 hover:bg-indigo-50 dark:hover:bg-gray-700'
                            }`}
                    >
                        <step.icon size={20} className={`mr-3 ${isActive ? 'text-white' : 'text-indigo-500 group-hover:text-indigo-600 dark:text-indigo-400 dark:group-hover:text-indigo-300'}`} />
                        <span className="text-sm font-medium">{step.id}. {step.title}</span>
                    </button>
                );
            })}
        </nav>
    );

    return (
        <div className="min-h-screen bg-gray-50 dark:bg-gray-900 font-sans transition-colors duration-500">
            <style>
                {`
                .scrollbar-thin::-webkit-scrollbar { width: 8px; }
                .scrollbar-thin::-webkit-scrollbar-thumb { background-color: #9ca3af; border-radius: 4px; }
                .dark .scrollbar-thin::-webkit-scrollbar-thumb { background-color: #4b5563; }
                `}
            </style>

            <ThemeToggle theme={theme} setTheme={setTheme} />

            <header className="bg-white dark:bg-gray-800 shadow-md sticky top-0 z-40">
                <div className="max-w-full mx-auto p-4 flex justify-between items-center">
                    <h1 className="text-xl md:text-2xl font-bold text-indigo-600 dark:text-indigo-400 flex items-center">
                        <Layers size={30} className="mr-2" /> MOD MED MAP
                    </h1>
                    {currentPage === 'dashboard' && (
                        <button
                            className="md:hidden p-2 rounded-lg text-gray-700 dark:text-gray-300 hover:bg-gray-100 dark:hover:bg-gray-700"
                            onClick={() => setIsSidebarOpen(!isSidebarOpen)}
                            aria-label="Toggle Navigation Menu"
                        >
                            <Menu size={24} />
                        </button>
                    )}
                     {currentPage === 'landing' && (
                        <button
                            onClick={() => setCurrentPage('dashboard')}
                            className="px-4 py-2 text-sm font-semibold bg-indigo-600 text-white rounded-lg hover:bg-indigo-700 transition"
                        >
                            Start Review
                        </button>
                    )}
                </div>
            </header>

            {currentPage === 'landing' && <LandingPage startReview={setCurrentPage} />}

            {currentPage === 'dashboard' && (
                <div className="flex flex-col md:flex-row max-w-full mx-auto">
                    {/* 1. Mobile Step Selector / Sidebar Drawer */}
                    {isSidebarOpen && (
                        <div className="md:hidden fixed inset-0 bg-black bg-opacity-50 z-30" onClick={() => setIsSidebarOpen(false)}>
                            <div className="bg-white dark:bg-gray-800 w-3/4 h-full overflow-y-auto shadow-2xl" onClick={(e) => e.stopPropagation()}>
                                <Sidebar />
                            </div>
                        </div>
                    )}

                    {/* 2. Desktop Sidebar */}
                    <div className="hidden md:block">
                        <Sidebar />
                    </div>

                    {/* 3. Main Content Area */}
                    <main className="flex-1 p-4 md:p-8 md:pt-12 min-w-0">
                        <h2 className="text-3xl font-extrabold text-gray-900 dark:text-white mb-6 hidden md:block">
                            {currentStepData.title}
                        </h2>

                        {/* Mobile Step Selector (Dropdown) */}
                        <div className="md:hidden mb-6">
                            <label className="block text-sm font-medium mb-2 text-gray-700 dark:text-gray-300">Current Phase:</label>
                            <select
                                value={currentStep}
                                onChange={(e) => handleStepChange(parseInt(e.target.value))}
                                className="w-full p-3 border rounded-lg dark:bg-gray-700 dark:border-gray-600 focus:ring-indigo-500 focus:border-indigo-500 text-gray-900 dark:text-white"
                            >
                                {STEPS.map(step => (
                                    <option key={step.id} value={step.id}>{step.id}. {step.title}</option>
                                ))}
                            </select>
                        </div>

                        {/* Step Content Card */}
                        <StepCard
                            step={currentStepData}
                            projectData={projectData}
                            setProjectData={setProjectData}
                            library={library}
                            setLibrary={setLibrary}
                        />

                    </main>
                </div>
            )}

            {/* AI Chatbot Assistant */}
            <ChatBotAssistant onChatStart={() => setIsChatOpen(true)} />
            <ChatModal isVisible={isChatOpen} onClose={() => setIsChatOpen(false)} theme={theme} />

            {/* Footer */}
            <footer className="mt-12 p-4 text-center text-sm text-gray-500 dark:text-gray-400 border-t dark:border-gray-700">
                <p>&copy; {new Date().getFullYear()} MOD MED MAP. Built with React & Tailwind CSS.</p>
                <p className="mt-1">Research Buddy powered by Gemini API.</p>
            </footer>

        </div>
    );
};

export default App;
