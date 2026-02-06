[index (1).html](https://github.com/user-attachments/files/25117423/index.1.html)# matheus-menezes-
portifolio 022
[App (1).tsx](https://github.com/user-attachments/files/25117405/App.1.tsx)

import React, { useState } from 'react';
import Sidebar from './components/Sidebar';
import Hero from './components/Hero';
import About from './components/About';[AIAssistant.tsx](https://github.com/user-a
import React from 'react';

interface HeaderProps {
  isOpen: boolean;
  toggleMenu: () => void;
}

const Header: React.FC<HeaderProps> = ({ isOpen, toggleMenu }) => {
  const navItems = [
    { label: 'Início', href: '#inicio' },
    { label: 'Sobre', href: '#sobre' },
    { label: 'Serviços', href: '#servicos' },
  ];

  return (
    <header className="fixed w-full top-0 z-50 glass border-b border-white/5">
      <div className="max-w-7xl mx-auto px-6 h-20 flex justify-between items-center">
        <div className="flex-shrink-0">
          <span className="text-xl font-black tracking-tighter text-white">
            DESIGNER<span className="text-neon">.</span>ESTRATÉGICO
          </span>
        </div>
        
        <nav className="hidden md:flex space-x-10">
          {navItems.map((item) => (
            <a
              key={item.label}
              href={item.href}
              className="text-slate-400 hover:text-neon transition-colors text-xs font-bold uppercase tracking-widest"
            >
              {item.label}
            </a>
          ))}
        </nav>

        <div className="hidden md:block">
          <a
            href="https://wa.me/seunumerodecelular"
            className="bg-neon hover:bg-emerald-500 text-black px-6 py-2.5 rounded-full text-xs font-black uppercase tracking-tighter transition-all hover:scale-105"
          >
            WhatsApp
          </a>
        </div>

        <button onClick={toggleMenu} className="md:hidden text-white">
          <svg className="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24">
            <path strokeLinecap="round" strokeLinejoin="round" strokeWidth={2} d={isOpen ? "M6 18L18 6M6 6l12 12" : "M4 6h16M4 12h16M4 18h16"} />
          </svg>
        </button>
      </div>

      {/* Mobile Menu */}
      {isOpen && (
        <div className="md:hidden glass border-b border-white/5 py-6">
          <div className="px-6 space-y-4 text-center">
            {navItems.map((item) => (
              <a key={item.label} href={item.href} onClick={toggleMenu} className="block text-slate-300 font-bold py-2">{item.label}</a>
            ))}
            <a href="https://wa.me/seunumerodecelular" className="block bg-neon text-black py-4 rounded-xl font-black">FALAR NO WHATSAPP</a>
          </div>
        </div>
      )}
    </header>
  );
};

export default Header;
[Header (1).tsx](https://github.com/user-attachments/files/25117409/Header.1.tsx)
ttachments/files/25117407/AIAssistant.tsx)
import React, { useState, useRef, useEffect } from 'react';
import { GoogleGenAI } from "@google/genai";[AIAssistant.tsx](https://github.com/user-attachments/files/25117408/AIAssistant.tsx)
import React, { useState, useRef, useEffect } from 'react';
import { GoogleGenAI } from "@google/genai";

const AIAssistant: React.FC = () => {
  const [isOpen, setIsOpen] = useState(false);
  const [input, setInput] = useState('');
  const [messages, setMessages] = useState<{ role: 'user' | 'ai', text: string }[]>([
    { role: 'ai', text: 'Olá! Sou o Estrategista AI. Como posso ajudar a transformar sua presença digital hoje?' }
  ]);
  const [isLoading, setIsLoading] = useState(false);
  const scrollRef = useRef<HTMLDivElement>(null);

  useEffect(() => {
    if (scrollRef.current) scrollRef.current.scrollTop = scrollRef.current.scrollHeight;
  }, [messages, isOpen]);

  const handleSend = async () => {
    if (!input.trim() || isLoading) return;
    const userMsg = input;
    setInput('');
    setMessages(prev => [...prev, { role: 'user', text: userMsg }]);
    setIsLoading(true);

    try {
      const ai = new GoogleGenAI({ apiKey: process.env.API_KEY });
      const response = await ai.models.generateContent({
        model: 'gemini-3-flash-preview',
        contents: [{ role: 'user', parts: [{ text: `Aja como um consultor de web design premium. Responda em PT-BR de forma executiva. Pergunta: ${userMsg}` }] }],
      });
      setMessages(prev => [...prev, { role: 'ai', text: response.text || "Vamos falar no WhatsApp?" }]);
    } catch (e) {
      setMessages(prev => [...prev, { role: 'ai', text: "Erro na conexão. Use o botão de WhatsApp!" }]);
    } finally {
      setIsLoading(false);
    }
  };

  return (
    <div className="fixed bottom-8 right-8 z-[60]">
      {isOpen ? (
        <div className="glass-card w-[350px] sm:w-[400px] h-[500px] rounded-[40px] border border-rose-500/20 shadow-2xl flex flex-col overflow-hidden animate-in slide-in-from-bottom duration-300">
          <div className="p-6 border-b border-white/5 flex justify-between items-center bg-accent/10">
            <span className="font-black text-[10px] uppercase tracking-widest text-white flex items-center gap-2">
              <span className="w-1.5 h-1.5 bg-accent rounded-full animate-ping"></span>
              Consultoria AI
            </span>
            <button onClick={() => setIsOpen(false)} className="text-zinc-500 hover:text-white">✕</button>
          </div>
          <div ref={scrollRef} className="flex-grow overflow-y-auto p-6 space-y-4 text-xs font-medium leading-relaxed">
            {messages.map((m, idx) => (
              <div key={idx} className={`flex ${m.role === 'user' ? 'justify-end' : 'justify-start'}`}>
                <div className={`max-w-[85%] p-4 rounded-[25px] ${m.role === 'user' ? 'bg-accent text-white font-bold' : 'bg-white/5 border border-white/10 text-zinc-300'}`}>
                  {m.text}
                </div>
              </div>
            ))}
            {isLoading && <div className="text-accent animate-pulse font-bold text-[10px] uppercase tracking-widest">Analisando Estratégia...</div>}
          </div>
          <div className="p-6 border-t border-white/5">
            <div className="relative">
              <input value={input} onChange={(e) => setInput(e.target.value)} onKeyDown={(e) => e.key === 'Enter' && handleSend()} placeholder="Sua dúvida estratégica..." className="w-full bg-white/5 border border-white/10 rounded-2xl py-4 pl-5 pr-14 focus:outline-none focus:border-accent transition-colors text-white text-xs" />
              <button onClick={handleSend} className="absolute right-2 top-2 p-2.5 bg-accent rounded-xl text-white">➤</button>
            </div>
          </div>
        </div>
      ) : (
        <button onClick={() => setIsOpen(true)} className="w-16 h-16 bg-accent rounded-[25px] flex items-center justify-center shadow-[0_10px_30px_rgba(225,29,72,0.4)] hover:scale-110 active:scale-95 transition-all group">
          <svg className="w-8 h-8 text-white" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path strokeLinecap="round" strokeLinejoin="round" strokeWidth={2} d="M8 10h.01M12 10h.01M16 10h.01M9 16H5a2 2 0 01-2-2V6a2 2 0 012-2h14a2 2 0 012 2v8a2 2 0 01-2 2h-5l-5 5v-5z" /></svg>
        </button>
      )}
    </div>
  );
};

export default AIAssistant;



const AIAssistant: React.FC = () => {
  const [isOpen, setIsOpen] = useState(false);
  const [input, setInput] = useState('');
  const [messages, setMessages] = useState<{ role: 'user' | 'ai', text: string }[]>([
    { role: 'ai', text: 'Olá! Sou o Estrategista AI. Como posso ajudar a transformar sua presença digital hoje?' }
  ]);
  const [isLoading, setIsLoading] = useState(false);
  const scrollRef = useRef<HTMLDivElement>(null);

  useEffect(() => {
    if (scrollRef.current) scrollRef.current.scrollTop = scrollRef.current.scrollHeight;
  }, [messages, isOpen]);

  const handleSend = async () => {
    if (!input.trim() || isLoading) return;
    const userMsg = input;
    setInput('');
    setMessages(prev => [...prev, { role: 'user', text: userMsg }]);
    setIsLoading(true);

    try {
      const ai = new GoogleGenAI({ apiKey: process.env.API_KEY });
      const response = await ai.models.generateContent({
        model: 'gemini-3-flash-preview',
        contents: [{ role: 'user', parts: [{ text: `Aja como um consultor de web design premium. Responda em PT-BR de forma executiva. Pergunta: ${userMsg}` }] }],
      });
      setMessages(prev => [...prev, { role: 'ai', text: response.text || "Vamos falar no WhatsApp?" }]);
    } catch (e) {
      setMessages(prev => [...prev, { role: 'ai', text: "Erro na conexão. Use o botão de WhatsApp!" }]);
    } finally {
      setIsLoading(false);
    }
  };

  return (
    <div className="fixed bottom-8 right-8 z-[60]">
      {isOpen ? (
        <div className="glass-card w-[350px] sm:w-[400px] h-[500px] rounded-[40px] border border-rose-500/20 shadow-2xl flex flex-col overflow-hidden animate-in slide-in-from-bottom duration-300">
          <div className="p-6 border-b border-white/5 flex justify-between items-center bg-accent/10">
            <span className="font-black text-[10px] uppercase tracking-widest text-white flex items-center gap-2">
              <span className="w-1.5 h-1.5 bg-accent rounded-full animate-ping"></span>
              Consultoria AI
            </span>
            <button onClick={() => setIsOpen(false)} className="text-zinc-500 hover:text-white">✕</button>
          </div>
          <div ref={scrollRef} className="flex-grow overflow-y-auto p-6 space-y-4 text-xs font-medium leading-relaxed">
            {messages.map((m, idx) => (
              <div key={idx} className={`flex ${m.role === 'user' ? 'justify-end' : 'justify-start'}`}>
                <div className={`max-w-[85%] p-4 rounded-[25px] ${m.role === 'user' ? 'bg-accent text-white font-bold' : 'bg-white/5 border border-white/10 text-zinc-300'}`}>
                  {m.text}
                </div>
              </div>
            ))}
            {isLoading && <div className="text-accent animate-pulse font-bold text-[10px] uppercase tracking-widest">Analisando Estratégia...</div>}
          </div>
          <div className="p-6 border-t border-white/5">
            <div className="relative">
              <input value={input} onChange={(e) => setInput(e.target.value)} onKeyDown={(e) => e.key === 'Enter' && handleSend()} placeholder="Sua dúvida estratégica..." className="w-full bg-white/5 border border-white/10 rounded-2xl py-4 pl-5 pr-14 focus:outline-none focus:border-accent transition-colors text-white text-xs" />
              <button onClick={handleSend} className="absolute right-2 top-2 p-2.5 bg-accent rounded-xl text-white">➤</button>
            </div>
          </div>
        </div>
      ) : (
        <button onClick={() => setIsOpen(true)} className="w-16 h-16 bg-accent rounded-[25px] flex items-center justify-center shadow-[0_10px_30px_rgba(225,29,72,0.4)] hover:scale-110 active:scale-95 transition-all group">
          <svg className="w-8 h-8 text-white" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path strokeLinecap="round" strokeLinejoin="round" strokeWidth={2} d="M8 10h.01M12 10h.01M16 10h.01M9 16H5a2 2 0 01-2-2V6a2 2 0 012-2h14a2 2 0 012 2v8a2 2 0 01-2 2h-5l-5 5v-5z" /></svg>
        </button>
      )}
    </div>
  );
};

export default AIAssistant;


import Services from './components/Services';
import Portfolio from './components/Portfolio';
import Testimonials from './components/Testimonials';
import Contact from './components/Contact';
import Footer from './components/Footer';
import AIAssistant from './components/AIAssistant';

const App: React.FC = () => {
  const [isSidebarOpen, setIsSidebarOpen] = useState(false);[Uploading index (1).html…
<!DOCTYPE html>
<html lang="pt-BR">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Designer Estratégico | Portfólio Premium</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@300;400;500;600;700;800&display=swap" rel="stylesheet">
    <style>
      :root {
        --accent: #e11d48;
        --bg-deep: #050505;
        --bg-card: rgba(15, 15, 15, 0.8);
      }
      body {
        font-family: 'Plus Jakarta Sans', sans-serif;
        scroll-behavior: smooth;
        background-color: var(--bg-deep);
        color: #e2e8f0;
        margin: 0;
      }
      .bg-accent { background-color: var(--accent); }
      .text-accent { color: var(--accent); }
      .border-accent { border-color: var(--accent); }
      
      .glass-card {
        background: var(--bg-card);
        backdrop-filter: blur(12px);
        border: 1px solid rgba(255, 255, 255, 0.05);
        transition: all 0.5s cubic-bezier(0.4, 0, 0.2, 1);
      }
      .sidebar-glass {
        background: rgba(8, 8, 8, 0.98);
        backdrop-filter: blur(20px);
        border-right: 1px solid rgba(255, 255, 255, 0.05);
      }
      .designer-block {
        background-color: var(--accent);
        color: #fff;
        display: inline-block;
        padding: 0.1em 0.4em;
        transform: skewX(-4deg);
        box-shadow: 10px 10px 0px rgba(0,0,0,0.3);
      }
      .gradient-text {
        background: linear-gradient(to right, #fff, #e11d48);
        -webkit-background-clip: text;
        -webkit-text-fill-color: transparent;
      }
      /* Scrollbar Customizada */
      ::-webkit-scrollbar { width: 6px; }
      ::-webkit-scrollbar-track { background: #050505; }
      ::-webkit-scrollbar-thumb { background: #222; border-radius: 10px; }
      ::-webkit-scrollbar-thumb:hover { background: var(--accent); }
      
      @keyframes float {
        0% { transform: translateY(0px); }
        50% { transform: translateY(-10px); }
        100% { transform: translateY(0px); }
      }
      .animate-float { animation: float 5s ease-in-out infinite; }
    </style>
    <script type="importmap">
    {
      "imports": {
        "react/": "https://esm.sh/react@^19.2.4/",
        "react": "https://esm.sh/react@^19.2.4",
        "@google/genai": "https://esm.sh/@google/genai@^1.40.0",
        "react-dom/": "https://esm.sh/react-dom@^19.2.4/"
      }
    }
    </script>
  </head>
  <body>
    <div id="root"></div>
  </body>
</html>
]()


  return (
    <div className="min-h-screen bg-[#050505] flex">
      {/* Sidebar de navegação fixa */}
      <Sidebar isOpen={isSidebarOpen} setIsOpen={setIsSidebarOpen} />

      {/* Área principal - Ajustada para o menu lateral no desktop */}
      <main className="flex-grow lg:ml-72 min-h-screen relative overflow-x-hidden">
        {/* Efeitos de luz de fundo cinematográficos */}
        <div className="absolute top-0 right-0 w-[60%] h-[50%] bg-rose-950/10 blur-[180px] rounded-full -z-10"></div>
        <div className="absolute bottom-[20%] left-[-10%] w-[40%] h-[40%] bg-zinc-900/20 blur-[150px] rounded-full -z-10"></div>

        <div className="max-w-6xl mx-auto px-6 lg:px-12">
          <Hero />
          <About />
          <Services />
          <Portfolio />
          <Testimonials />
          <Contact />
          <Footer />
        </div>
      </main>

      <AIAssistant />
    </div>
  );
};

export default App;
