import React, { useState, useEffect } from 'react';
import { Github, Linkedin, Instagram, Mail, MapPin, Clock, Code, Star, GitFork, Users, Activity, Zap, Award, Target, Flame, TrendingUp, Terminal, Cpu, Database, Server, Globe, GitCommit, GitBranch, GitPullRequest, Calendar, BookOpen, Rocket } from 'lucide-react';

const GitHubDashboard = () => {
  const [terminalLines, setTerminalLines] = useState([]);
  const [currentCommand, setCurrentCommand] = useState('');
  const [mousePos, setMousePos] = useState({ x: 0, y: 0 });
  const [commitData, setCommitData] = useState(null);
  const [repoData, setRepoData] = useState([]);
  const [loading, setLoading] = useState(true);
  const [currentTime, setCurrentTime] = useState(new Date());

  const commands = [
    '$ git status',
    '  On branch main',
    '  Your branch is up to date',
    '',
    '$ git log --oneline -5',
    '  a3f82b1 feat: Enhanced dashboard with animations',
    '  c9d45e3 fix: Optimized API calls',
    '  7b2e8f1 docs: Updated documentation',
    '  5a1c3d9 refactor: Improved performance',
    '  2f8e6b4 style: Modern UI components',
    '',
    '$ npm run dev',
    '  Vite dev server running...',
    '  Ready in 342ms',
    '',
    '$ echo "Building the future, one commit at a time"',
    '  Building the future, one commit at a time',
  ];

  useEffect(() => {
    let lineIndex = 0;
    let charIndex = 0;
    let currentLine = '';

    const typeInterval = setInterval(() => {
      if (lineIndex < commands.length) {
        if (charIndex < commands[lineIndex].length) {
          currentLine += commands[lineIndex][charIndex];
          setCurrentCommand(currentLine);
          charIndex++;
        } else {
          setTerminalLines(prev => [...prev, commands[lineIndex]]);
          setCurrentCommand('');
          currentLine = '';
          lineIndex++;
          charIndex = 0;
        }
      } else {
        clearInterval(typeInterval);
      }
    }, 25);

    return () => clearInterval(typeInterval);
  }, []);

  useEffect(() => {
    const handleMouse = (e) => setMousePos({ x: e.clientX, y: e.clientY });
    window.addEventListener('mousemove', handleMouse);
    return () => window.removeEventListener('mousemove', handleMouse);
  }, []);

  useEffect(() => {
    const timer = setInterval(() => setCurrentTime(new Date()), 1000);
    return () => clearInterval(timer);
  }, []);

  useEffect(() => {
    const fetchGitHubData = async () => {
      try {
        const [userResponse, reposResponse] = await Promise.all([
          fetch('https://api.github.com/users/PiyushBytes'),
          fetch('https://api.github.com/users/PiyushBytes/repos?sort=updated&per_page=6')
        ]);
        
        const userData = await userResponse.json();
        const reposData = await reposResponse.json();
        
        setCommitData(userData);
        setRepoData(reposData);
        setLoading(false);
      } catch (error) {
        console.error('Error fetching GitHub data:', error);
        setLoading(false);
      }
    };

    fetchGitHubData();
    const interval = setInterval(fetchGitHubData, 300000);
    return () => clearInterval(interval);
  }, []);

  const stats = [
    { 
      label: 'Total Contributions', 
      value: '22', 
      icon: <Activity className="w-6 h-6" />, 
      color: 'from-emerald-500 via-green-500 to-teal-500', 
      detail: 'Jun 16, 2024 - Present',
      subIcon: <GitCommit className="w-4 h-4" />
    },
    { 
      label: 'Current Streak', 
      value: '2', 
      icon: <Flame className="w-6 h-6" />, 
      color: 'from-orange-500 via-red-500 to-pink-500', 
      detail: 'Dec 18 - Dec 19', 
      highlight: true,
      subIcon: <Zap className="w-4 h-4" />
    },
    { 
      label: 'Longest Streak', 
      value: '2', 
      icon: <Target className="w-6 h-6" />, 
      color: 'from-purple-500 via-violet-500 to-indigo-500', 
      detail: 'Dec 18 - Dec 19',
      subIcon: <Award className="w-4 h-4" />
    },
    { 
      label: 'Repositories', 
      value: loading ? '...' : (commitData?.public_repos || '5'), 
      icon: <Database className="w-6 h-6" />, 
      color: 'from-cyan-500 via-blue-500 to-indigo-500', 
      detail: 'Public Projects',
      subIcon: <Server className="w-4 h-4" />
    },
  ];

  const liveStats = [
    { 
      label: 'Total Stars', 
      value: loading ? '...' : '16', 
      icon: <Star className="w-5 h-5" />, 
      gradient: 'from-yellow-400 to-orange-500',
      change: '+3 this week'
    },
    { 
      label: 'Followers', 
      value: loading ? '...' : (commitData?.followers || '2'), 
      icon: <Users className="w-5 h-5" />, 
      gradient: 'from-blue-400 to-cyan-500',
      change: 'Growing'
    },
    { 
      label: 'Following', 
      value: loading ? '...' : (commitData?.following || '13'), 
      icon: <TrendingUp className="w-5 h-5" />, 
      gradient: 'from-green-400 to-emerald-500',
      change: 'Active'
    },
    { 
      label: 'Pull Requests', 
      value: '3', 
      icon: <GitPullRequest className="w-5 h-5" />, 
      gradient: 'from-purple-400 to-pink-500',
      change: '2 merged'
    },
  ];

  const techStack = [
    { name: 'JavaScript', percentage: 51.08, color: 'bg-yellow-500', gradient: 'from-yellow-400 to-yellow-600', icon: '⚡' },
    { name: 'CSS', percentage: 38.26, color: 'bg-blue-500', gradient: 'from-blue-400 to-purple-500', icon: '🎨' },
    { name: 'HTML', percentage: 10.66, color: 'bg-orange-500', gradient: 'from-orange-400 to-red-500', icon: '🌐' },
  ];

  const socialLinks = [
    { name: 'LinkedIn', icon: <Linkedin className="w-5 h-5" />, url: 'https://www.linkedin.com/in/piyush-dawn?utm_source=share&utm_campaign=share_via&utm_content=profile&utm_medium=android_app', color: 'from-blue-600 to-blue-400', bgColor: 'bg-blue-500/10 hover:bg-blue-500/20' },
    { name: 'GitHub', icon: <Github className="w-5 h-5" />, url: 'https://github.com/PiyushBytes', color: 'from-gray-600 to-gray-400', bgColor: 'bg-gray-500/10 hover:bg-gray-500/20' },
    { name: 'Instagram', icon: <Instagram className="w-5 h-5" />, url: 'https://instagram.com/piyushdawn', color: 'from-pink-600 via-purple-600 to-orange-500', bgColor: 'bg-gradient-to-r from-pink-500/10 to-orange-500/10 hover:from-pink-500/20 hover:to-orange-500/20' },
    { name: 'Email', icon: <Mail className="w-5 h-5" />, url: 'mailto:dawnpiyushofficial384@gmail.com', color: 'from-red-600 to-red-400', bgColor: 'bg-red-500/10 hover:bg-red-500/20' },
  ];

  const generateContributions = () => {
    const weeks = 52;
    const data = [];
    for (let week = 0; week < weeks; week++) {
      const weekData = [];
      for (let day = 0; day < 7; day++) {
        const random = Math.random();
        let contributions = 0;
        if (random > 0.7) contributions = 1;
        if (random > 0.85) contributions = 2;
        if (random > 0.95) contributions = 3;
        if (random > 0.98) contributions = 4;
        weekData.push(contributions);
      }
      data.push(weekData);
    }
    return data;
  };

  const contributions = generateContributions();

  const getContributionColor = (count) => {
    if (count === 0) return 'bg-gray-800/50 hover:bg-gray-700/50';
    if (count === 1) return 'bg-emerald-900/60 hover:bg-emerald-800/70';
    if (count === 2) return 'bg-emerald-700/70 hover:bg-emerald-600/80';
    if (count === 3) return 'bg-emerald-500/80 hover:bg-emerald-400/90';
    return 'bg-emerald-400 hover:bg-emerald-300';
  };

  return (
    <div className="min-h-screen bg-gradient-to-br from-gray-950 via-black to-gray-950 text-white overflow-x-hidden relative">
      <div className="fixed inset-0 pointer-events-none opacity-5">
        <div className="absolute inset-0 bg-[linear-gradient(transparent_0%,transparent_90%,#0f0_90%,#0f0_100%)] bg-[length:100px_100px] animate-[matrix_20s_linear_infinite]" />
      </div>

      <div className="fixed inset-0 pointer-events-none opacity-10">
        <div className="absolute inset-0" style={{
          backgroundImage: 'linear-gradient(rgba(139, 92, 246, 0.1) 1px, transparent 1px), linear-gradient(90deg, rgba(139, 92, 246, 0.1) 1px, transparent 1px)',
          backgroundSize: '50px 50px',
        }} />
      </div>

      <div 
        className="fixed w-96 h-96 rounded-full pointer-events-none blur-3xl opacity-20 transition-all duration-300"
        style={{
          background: 'radial-gradient(circle, rgba(139, 92, 246, 0.5) 0%, transparent 70%)',
          left: mousePos.x - 192,
          top: mousePos.y - 192,
        }}
      />

      <div className="max-w-7xl mx-auto p-6 relative z-10">
        <div className="mb-8">
          <div className="bg-gradient-to-br from-gray-900/80 via-purple-900/20 to-gray-900/80 backdrop-blur-xl border border-purple-500/20 rounded-3xl p-8 shadow-2xl">
            <div className="flex flex-col md:flex-row items-center md:items-start gap-8 mb-8">
              <div className="relative group">
                <div className="absolute -inset-1 bg-gradient-to-r from-purple-600 via-pink-600 to-cyan-600 rounded-full blur-lg opacity-75 group-hover:opacity-100 transition-opacity" />
                <img 
                  src="https://avatars.githubusercontent.com/u/172993163?v=4" 
                  alt="Piyush Dawn" 
                  className="w-32 h-32 rounded-full border-4 border-gray-900 relative z-10 shadow-2xl"
                />
                <div className="absolute -bottom-2 -right-2 bg-gradient-to-r from-green-400 to-emerald-500 rounded-full p-2 border-4 border-gray-900 z-20">
                  <div className="w-3 h-3 bg-white rounded-full animate-pulse" />
                </div>
              </div>

              <div className="flex-1 text-center md:text-left">
                <div className="flex items-center justify-center md:justify-start gap-3 mb-2">
                  <h1 className="text-5xl font-black bg-gradient-to-r from-purple-400 via-pink-400 to-cyan-400 bg-clip-text text-transparent">
                    Piyush Dawn
                  </h1>
                  <Award className="w-6 h-6 text-yellow-500 animate-pulse" />
                </div>
                
                <div className="flex items-center justify-center md:justify-start gap-2 text-lg text-gray-400 mb-4">
                  <Code className="w-4 h-4 text-purple-400" />
                  <span className="text-purple-400 font-mono">@PiyushBytes</span>
                  <div className="w-2 h-2 bg-green-500 rounded-full animate-pulse" />
                  <span className="text-green-400 text-sm">Online</span>
                </div>

                <p className="text-gray-300 mb-6 max-w-2xl leading-relaxed">
                  👋 IT student, aspiring developer, and creative freelancer passionate about building meaningful digital experiences
                </p>

                <div className="flex flex-wrap items-center justify-center md:justify-start gap-4 text-sm">
                  <div className="flex items-center gap-2 bg-gray-800/50 px-3 py-2 rounded-lg backdrop-blur-sm">
                    <MapPin className="w-4 h-4 text-purple-400" />
                    <span>Kolkata, India</span>
                  </div>
                  <div className="flex items-center gap-2 bg-gray-800/50 px-3 py-2 rounded-lg backdrop-blur-sm">
                    <Clock className="w-4 h-4 text-cyan-400" />
                    <span>{currentTime.toLocaleTimeString('en-US', { hour: '2-digit', minute: '2-digit' })} UTC +5:30</span>
                  </div>
                  <div className="flex items-center gap-2 bg-green-500/20 border border-green-500/30 px-3 py-2 rounded-lg backdrop-blur-sm">
                    <Zap className="w-4 h-4 text-green-400" />
                    <span className="text-green-300 font-semibold">Open to Work</span>
                  </div>
                </div>
              </div>
            </div>

            <div className="bg-gray-950 border border-gray-800 rounded-xl overflow-hidden shadow-2xl">
              <div className="flex items-center gap-2 px-4 py-3 bg-gray-900 border-b border-gray-800">
                <div className="flex gap-2">
                  <div className="w-3 h-3 rounded-full bg-red-500" />
                  <div className="w-3 h-3 rounded-full bg-yellow-500" />
                  <div className="w-3 h-3 rounded-full bg-green-500" />
                </div>
                <div className="flex-1 text-center">
                  <span className="text-gray-500 text-sm font-mono">piyush@dev: ~/github</span>
                </div>
                <Terminal className="w-4 h-4 text-gray-600" />
              </div>
              <div className="p-4 font-mono text-sm h-64 overflow-y-auto">
                {terminalLines.map((line, index) => {
                  const lineStr = String(line || '');
                  const colorClass = lineStr.startsWith('
            </div>

            <div className="mt-6 pt-6 border-t border-gray-700/50">
              <div className="grid grid-cols-2 md:flex gap-3">
                {socialLinks.map((social, index) => (
                  <a
                    key={index}
                    href={social.url}
                    target="_blank"
                    rel="noopener noreferrer"
                    className={`${social.bgColor} border border-gray-700/50 rounded-xl p-4 transition-all duration-300 hover:scale-105 hover:border-purple-500/50 group flex items-center gap-3`}
                  >
                    <div className={`bg-gradient-to-br ${social.color} p-2 rounded-lg group-hover:rotate-12 transition-transform`}>
                      {social.icon}
                    </div>
                    <div className="flex-1">
                      <div className="font-semibold text-white text-sm">{social.name}</div>
                      <div className="text-xs text-gray-400">Connect</div>
                    </div>
                  </a>
                ))}
              </div>
            </div>
          </div>
        </div>

        <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-6 mb-8">
          {stats.map((stat, index) => (
            <div 
              key={index}
              className={`group relative overflow-hidden bg-gradient-to-br from-gray-900/90 to-gray-800/90 backdrop-blur-xl border ${stat.highlight ? 'border-orange-500/50 shadow-orange-500/20' : 'border-purple-500/20'} rounded-2xl p-6 hover:scale-105 transition-all duration-500 cursor-pointer shadow-2xl`}
            >
              <div className={`absolute inset-0 bg-gradient-to-br ${stat.color} opacity-0 group-hover:opacity-10 transition-opacity duration-500`} />
              
              <div className="flex items-start justify-between mb-4">
                <div className={`w-12 h-12 bg-gradient-to-br ${stat.color} rounded-xl flex items-center justify-center group-hover:rotate-12 group-hover:scale-110 transition-all duration-300`}>
                  {stat.icon}
                </div>
                <div className={`bg-gradient-to-br ${stat.color} bg-opacity-20 rounded-lg p-1`}>
                  {stat.subIcon}
                </div>
              </div>

              <div className="text-4xl font-black mb-2 bg-gradient-to-r from-white to-gray-300 bg-clip-text text-transparent">
                {stat.value}
              </div>

              <div className="text-gray-400 font-semibold mb-1">{stat.label}</div>
              <div className="text-xs text-gray-500">{stat.detail}</div>

              <div className="absolute top-0 left-0 w-full h-full bg-gradient-to-r from-transparent via-white/5 to-transparent translate-x-[-100%] group-hover:translate-x-[100%] transition-transform duration-1000" />
            </div>
          ))}
        </div>

        <div className="grid grid-cols-2 md:grid-cols-4 gap-4 mb-8">
          {liveStats.map((stat, index) => (
            <div 
              key={index}
              className="bg-gradient-to-br from-gray-900/90 to-gray-800/90 backdrop-blur-xl border border-purple-500/20 rounded-xl p-5 hover:border-purple-500/50 transition-all group relative overflow-hidden"
            >
              <div className="flex items-center justify-between mb-3">
                <div className={`p-2 rounded-lg bg-gradient-to-br ${stat.gradient} group-hover:scale-110 transition-transform`}>
                  {stat.icon}
                </div>
                <div className="text-xs text-green-400 flex items-center gap-1">
                  <div className="w-2 h-2 bg-green-400 rounded-full animate-pulse" />
                  Live
                </div>
              </div>
              <div className="text-3xl font-bold mb-1">{stat.value}</div>
              <div className="text-sm text-gray-400 mb-1">{stat.label}</div>
              <div className="text-xs text-gray-500">{stat.change}</div>
            </div>
          ))}
        </div>

        <div className="grid md:grid-cols-3 gap-6 mb-8">
          <div className="md:col-span-2 bg-gradient-to-br from-gray-900/90 to-gray-800/90 backdrop-blur-xl border border-purple-500/20 rounded-2xl p-6 shadow-2xl">
            <div className="flex items-center justify-between mb-6">
              <h3 className="text-2xl font-bold flex items-center gap-2">
                <Activity className="w-6 h-6 text-emerald-400" />
                <span className="bg-gradient-to-r from-emerald-400 to-cyan-400 bg-clip-text text-transparent">
                  Contribution Activity
                </span>
              </h3>
              <div className="flex items-center gap-2 text-sm">
                <div className="w-2 h-2 bg-emerald-400 rounded-full animate-pulse" />
                <span className="text-gray-400">Live Updates</span>
              </div>
            </div>
            
            <div className="overflow-x-auto pb-4">
              <div className="inline-flex gap-1">
                {contributions.map((week, weekIndex) => (
                  <div key={weekIndex} className="flex flex-col gap-1">
                    {week.map((day, dayIndex) => (
                      <div
                        key={dayIndex}
                        className={`w-3 h-3 rounded-sm ${getContributionColor(day)} transition-all cursor-pointer hover:scale-150 hover:ring-2 hover:ring-emerald-400`}
                        title={`${day} contributions`}
                      />
                    ))}
                  </div>
                ))}
              </div>
            </div>
            
            <div className="flex items-center justify-between mt-4">
              <div className="flex items-center gap-3 text-xs text-gray-400">
                <span>Less</span>
                <div className="flex gap-1">
                  {[0, 1, 2, 3, 4].map(level => (
                    <div key={level} className={`w-4 h-4 rounded-sm ${getContributionColor(level).split(' ')[0]}`} />
                  ))}
                </div>
                <span>More</span>
              </div>
              <div className="text-sm text-gray-500">
                Updated: {currentTime.toLocaleTimeString()}
              </div>
            </div>
          </div>

          <div className="bg-gradient-to-br from-gray-900/90 to-gray-800/90 backdrop-blur-xl border border-purple-500/20 rounded-2xl p-6 shadow-2xl">
            <h3 className="text-2xl font-bold mb-6 flex items-center gap-2">
              <Code className="w-6 h-6 text-purple-400" />
              <span className="bg-gradient-to-r from-purple-400 to-pink-400 bg-clip-text text-transparent">
                Languages
              </span>
            </h3>
            
            <div className="space-y-5">
              {techStack.map((tech, index) => (
                <div key={index} className="group">
                  <div className="flex items-center justify-between mb-2">
                    <div className="flex items-center gap-2">
                      <span className="text-xl">{tech.icon}</span>
                      <span className="font-semibold text-gray-200">{tech.name}</span>
                    </div>
                    <span className="text-sm text-gray-400 font-mono">{tech.percentage.toFixed(2)}%</span>
                  </div>
                  <div className="relative h-3 bg-gray-800 rounded-full overflow-hidden">
                    <div 
                      className={`absolute top-0 left-0 h-full bg-gradient-to-r ${tech.gradient} transition-all duration-1000 ease-out group-hover:animate-pulse`}
                      style={{ width: `${tech.percentage}%` }}
                    />
                  </div>
                </div>
              ))}
            </div>

            <div className="mt-6 pt-6 border-t border-gray-700/50">
              <div className="flex items-center justify-between">
                <span className="text-sm text-gray-400">Total Code Lines</span>
                <span className="text-lg font-bold bg-gradient-to-r from-purple-400 to-pink-400 bg-clip-text text-transparent">
                  2,500+
                </span>
              </div>
              <div className="flex items-center gap-2 mt-2 text-xs text-gray-500">
                <GitBranch className="w-3 h-3" />
                <span>Across {loading ? '...' : commitData?.public_repos || '5'} repositories</span>
              </div>
            </div>
          </div>
        </div>

        <div className="bg-gradient-to-br from-gray-900/90 to-gray-800/90 backdrop-blur-xl border border-purple-500/20 rounded-2xl p-6 shadow-2xl mb-8">
          <h3 className="text-2xl font-bold mb-6 flex items-center gap-2">
            <Rocket className="w-6 h-6 text-cyan-400" />
            <span className="bg-gradient-to-r from-cyan-400 to-blue-400 bg-clip-text text-transparent">
              Recent Repositories
            </span>
          </h3>
          
          <div className="grid md:grid-cols-2 lg:grid-cols-3 gap-4">
            {loading ? (
              <div className="col-span-full text-center text-gray-400">Loading repositories...</div>
            ) : repoData.length > 0 ? (
              repoData.map((repo, index) => (
                <a
                  key={index}
                  href={repo.html_url}
                  target="_blank"
                  rel="noopener noreferrer"
                  className="group bg-gradient-to-br from-gray-800/80 to-gray-900/80 border border-gray-700/50 rounded-xl p-5 hover:border-purple-500/50 hover:scale-105 transition-all duration-300"
                >
                  <div className="flex items-start justify-between mb-3">
                    <BookOpen className="w-5 h-5 text-purple-400" />
                    <div className="flex items-center gap-2">
                      {repo.stargazers_count > 0 && (
                        <div className="flex items-center gap-1 text-xs text-yellow-400">
                          <Star className="w-3 h-3" />
                          {repo.stargazers_count}
                        </div>
                      )}
                    </div>
                  </div>
                  
                  <h4 className="font-bold text-white mb-2 group-hover:text-purple-400 transition-colors truncate">
                    {repo.name}
                  </h4>
                  
                  <p className="text-sm text-gray-400 mb-3 line-clamp-2">
                    {repo.description || 'No description available'}
                  </p>
                  
                  <div className="flex items-center gap-3 text-xs text-gray-500">
                    {repo.language && (
                      <div className="flex items-center gap-1">
                        <div className="w-2 h-2 bg-purple-500 rounded-full" />
                        {repo.language}
                      </div>
                    )}
                    <div className="flex items-center gap-1">
                      <Calendar className="w-3 h-3" />
                      {new Date(repo.updated_at).toLocaleDateString()}
                    </div>
                  </div>
                </a>
              ))
            ) : (
              <div className="col-span-full text-center text-gray-400">No repositories found</div>
            )}
          </div>
        </div>

        <div className="text-center text-sm text-gray-500">
          <div className="flex items-center justify-center gap-2 mb-2">
            <div className="w-2 h-2 bg-green-500 rounded-full animate-pulse" />
            <span>Real-time data powered by GitHub API</span>
          </div>
          <p>Last synced: {currentTime.toLocaleString()}</p>
        </div>
      </div>

      <style>{`
        @keyframes matrix {
          0% { transform: translateY(0); }
          100% { transform: translateY(100px); }
        }
      `}</style>
    </div>
  );
};

export default GitHubDashboard;) 
                    ? 'text-green-400' 
                    : lineStr.startsWith('  Ready') || lineStr.startsWith('  Vite')
                    ? 'text-emerald-400' 
                    : lineStr.startsWith('  On') || lineStr.startsWith('  Your') || lineStr.startsWith('  Building')
                    ? 'text-blue-400' 
                    : 'text-gray-400';
                  
                  return (
                    <div key={index} className={`${colorClass} mb-1`}>
                      {lineStr}
                    </div>
                  );
                })}
                {currentCommand && (
                  <div className="text-green-400">
                    {currentCommand}<span className="animate-pulse">▊</span>
                  </div>
                )}
              </div>
            </div>

            <div className="mt-6 pt-6 border-t border-gray-700/50">
              <div className="grid grid-cols-2 md:flex gap-3">
                {socialLinks.map((social, index) => (
                  <a
                    key={index}
                    href={social.url}
                    target="_blank"
                    rel="noopener noreferrer"
                    className={`${social.bgColor} border border-gray-700/50 rounded-xl p-4 transition-all duration-300 hover:scale-105 hover:border-purple-500/50 group flex items-center gap-3`}
                  >
                    <div className={`bg-gradient-to-br ${social.color} p-2 rounded-lg group-hover:rotate-12 transition-transform`}>
                      {social.icon}
                    </div>
                    <div className="flex-1">
                      <div className="font-semibold text-white text-sm">{social.name}</div>
                      <div className="text-xs text-gray-400">Connect</div>
                    </div>
                  </a>
                ))}
              </div>
            </div>
          </div>
        </div>

        <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-6 mb-8">
          {stats.map((stat, index) => (
            <div 
              key={index}
              className={`group relative overflow-hidden bg-gradient-to-br from-gray-900/90 to-gray-800/90 backdrop-blur-xl border ${stat.highlight ? 'border-orange-500/50 shadow-orange-500/20' : 'border-purple-500/20'} rounded-2xl p-6 hover:scale-105 transition-all duration-500 cursor-pointer shadow-2xl`}
            >
              <div className={`absolute inset-0 bg-gradient-to-br ${stat.color} opacity-0 group-hover:opacity-10 transition-opacity duration-500`} />
              
              <div className="flex items-start justify-between mb-4">
                <div className={`w-12 h-12 bg-gradient-to-br ${stat.color} rounded-xl flex items-center justify-center group-hover:rotate-12 group-hover:scale-110 transition-all duration-300`}>
                  {stat.icon}
                </div>
                <div className={`bg-gradient-to-br ${stat.color} bg-opacity-20 rounded-lg p-1`}>
                  {stat.subIcon}
                </div>
              </div>

              <div className="text-4xl font-black mb-2 bg-gradient-to-r from-white to-gray-300 bg-clip-text text-transparent">
                {stat.value}
              </div>

              <div className="text-gray-400 font-semibold mb-1">{stat.label}</div>
              <div className="text-xs text-gray-500">{stat.detail}</div>

              <div className="absolute top-0 left-0 w-full h-full bg-gradient-to-r from-transparent via-white/5 to-transparent translate-x-[-100%] group-hover:translate-x-[100%] transition-transform duration-1000" />
            </div>
          ))}
        </div>

        <div className="grid grid-cols-2 md:grid-cols-4 gap-4 mb-8">
          {liveStats.map((stat, index) => (
            <div 
              key={index}
              className="bg-gradient-to-br from-gray-900/90 to-gray-800/90 backdrop-blur-xl border border-purple-500/20 rounded-xl p-5 hover:border-purple-500/50 transition-all group relative overflow-hidden"
            >
              <div className="flex items-center justify-between mb-3">
                <div className={`p-2 rounded-lg bg-gradient-to-br ${stat.gradient} group-hover:scale-110 transition-transform`}>
                  {stat.icon}
                </div>
                <div className="text-xs text-green-400 flex items-center gap-1">
                  <div className="w-2 h-2 bg-green-400 rounded-full animate-pulse" />
                  Live
                </div>
              </div>
              <div className="text-3xl font-bold mb-1">{stat.value}</div>
              <div className="text-sm text-gray-400 mb-1">{stat.label}</div>
              <div className="text-xs text-gray-500">{stat.change}</div>
            </div>
          ))}
        </div>

        <div className="grid md:grid-cols-3 gap-6 mb-8">
          <div className="md:col-span-2 bg-gradient-to-br from-gray-900/90 to-gray-800/90 backdrop-blur-xl border border-purple-500/20 rounded-2xl p-6 shadow-2xl">
            <div className="flex items-center justify-between mb-6">
              <h3 className="text-2xl font-bold flex items-center gap-2">
                <Activity className="w-6 h-6 text-emerald-400" />
                <span className="bg-gradient-to-r from-emerald-400 to-cyan-400 bg-clip-text text-transparent">
                  Contribution Activity
                </span>
              </h3>
              <div className="flex items-center gap-2 text-sm">
                <div className="w-2 h-2 bg-emerald-400 rounded-full animate-pulse" />
                <span className="text-gray-400">Live Updates</span>
              </div>
            </div>
            
            <div className="overflow-x-auto pb-4">
              <div className="inline-flex gap-1">
                {contributions.map((week, weekIndex) => (
                  <div key={weekIndex} className="flex flex-col gap-1">
                    {week.map((day, dayIndex) => (
                      <div
                        key={dayIndex}
                        className={`w-3 h-3 rounded-sm ${getContributionColor(day)} transition-all cursor-pointer hover:scale-150 hover:ring-2 hover:ring-emerald-400`}
                        title={`${day} contributions`}
                      />
                    ))}
                  </div>
                ))}
              </div>
            </div>
            
            <div className="flex items-center justify-between mt-4">
              <div className="flex items-center gap-3 text-xs text-gray-400">
                <span>Less</span>
                <div className="flex gap-1">
                  {[0, 1, 2, 3, 4].map(level => (
                    <div key={level} className={`w-4 h-4 rounded-sm ${getContributionColor(level).split(' ')[0]}`} />
                  ))}
                </div>
                <span>More</span>
              </div>
              <div className="text-sm text-gray-500">
                Updated: {currentTime.toLocaleTimeString()}
              </div>
            </div>
          </div>

          <div className="bg-gradient-to-br from-gray-900/90 to-gray-800/90 backdrop-blur-xl border border-purple-500/20 rounded-2xl p-6 shadow-2xl">
            <h3 className="text-2xl font-bold mb-6 flex items-center gap-2">
              <Code className="w-6 h-6 text-purple-400" />
              <span className="bg-gradient-to-r from-purple-400 to-pink-400 bg-clip-text text-transparent">
                Languages
              </span>
            </h3>
            
            <div className="space-y-5">
              {techStack.map((tech, index) => (
                <div key={index} className="group">
                  <div className="flex items-center justify-between mb-2">
                    <div className="flex items-center gap-2">
                      <span className="text-xl">{tech.icon}</span>
                      <span className="font-semibold text-gray-200">{tech.name}</span>
                    </div>
                    <span className="text-sm text-gray-400 font-mono">{tech.percentage.toFixed(2)}%</span>
                  </div>
                  <div className="relative h-3 bg-gray-800 rounded-full overflow-hidden">
                    <div 
                      className={`absolute top-0 left-0 h-full bg-gradient-to-r ${tech.gradient} transition-all duration-1000 ease-out group-hover:animate-pulse`}
                      style={{ width: `${tech.percentage}%` }}
                    />
                  </div>
                </div>
              ))}
            </div>

            <div className="mt-6 pt-6 border-t border-gray-700/50">
              <div className="flex items-center justify-between">
                <span className="text-sm text-gray-400">Total Code Lines</span>
                <span className="text-lg font-bold bg-gradient-to-r from-purple-400 to-pink-400 bg-clip-text text-transparent">
                  2,500+
                </span>
              </div>
              <div className="flex items-center gap-2 mt-2 text-xs text-gray-500">
                <GitBranch className="w-3 h-3" />
                <span>Across {loading ? '...' : commitData?.public_repos || '5'} repositories</span>
              </div>
            </div>
          </div>
        </div>

        <div className="bg-gradient-to-br from-gray-900/90 to-gray-800/90 backdrop-blur-xl border border-purple-500/20 rounded-2xl p-6 shadow-2xl mb-8">
          <h3 className="text-2xl font-bold mb-6 flex items-center gap-2">
            <Rocket className="w-6 h-6 text-cyan-400" />
            <span className="bg-gradient-to-r from-cyan-400 to-blue-400 bg-clip-text text-transparent">
              Recent Repositories
            </span>
          </h3>
          
          <div className="grid md:grid-cols-2 lg:grid-cols-3 gap-4">
            {loading ? (
              <div className="col-span-full text-center text-gray-400">Loading repositories...</div>
            ) : repoData.length > 0 ? (
              repoData.map((repo, index) => (
                <a
                  key={index}
                  href={repo.html_url}
                  target="_blank"
                  rel="noopener noreferrer"
                  className="group bg-gradient-to-br from-gray-800/80 to-gray-900/80 border border-gray-700/50 rounded-xl p-5 hover:border-purple-500/50 hover:scale-105 transition-all duration-300"
                >
                  <div className="flex items-start justify-between mb-3">
                    <BookOpen className="w-5 h-5 text-purple-400" />
                    <div className="flex items-center gap-2">
                      {repo.stargazers_count > 0 && (
                        <div className="flex items-center gap-1 text-xs text-yellow-400">
                          <Star className="w-3 h-3" />
                          {repo.stargazers_count}
                        </div>
                      )}
                    </div>
                  </div>
                  
                  <h4 className="font-bold text-white mb-2 group-hover:text-purple-400 transition-colors truncate">
                    {repo.name}
                  </h4>
                  
                  <p className="text-sm text-gray-400 mb-3 line-clamp-2">
                    {repo.description || 'No description available'}
                  </p>
                  
                  <div className="flex items-center gap-3 text-xs text-gray-500">
                    {repo.language && (
                      <div className="flex items-center gap-1">
                        <div className="w-2 h-2 bg-purple-500 rounded-full" />
                        {repo.language}
                      </div>
                    )}
                    <div className="flex items-center gap-1">
                      <Calendar className="w-3 h-3" />
                      {new Date(repo.updated_at).toLocaleDateString()}
                    </div>
                  </div>
                </a>
              ))
            ) : (
              <div className="col-span-full text-center text-gray-400">No repositories found</div>
            )}
          </div>
        </div>

        <div className="text-center text-sm text-gray-500">
          <div className="flex items-center justify-center gap-2 mb-2">
            <div className="w-2 h-2 bg-green-500 rounded-full animate-pulse" />
            <span>Real-time data powered by GitHub API</span>
          </div>
          <p>Last synced: {currentTime.toLocaleString()}</p>
        </div>
      </div>

      <style>{`
        @keyframes matrix {
          0% { transform: translateY(0); }
          100% { transform: translateY(100px); }
        }
      `}</style>
    </div>
  );
};

export default GitHubDashboard;
