# Horoscope
import { useState } from 'react';
import { Card, CardContent } from '@/components/ui/card';
import { Button } from '@/components/ui/button';
import { motion } from 'framer-motion';
import { Switch } from '@/components/ui/switch';

// Tailwind classes updated for dark mode support with stars
// Add these global styles in your tailwind.config.js or a CSS file if needed:
// .bg-stars {
//   background-image: url('/stars.svg');
//   background-repeat: repeat;
//   background-size: cover;
//   background-position: center;
// }

const zodiacEmojis = {
  Aries: '🐏',
  Taurus: '🐂',
  Gemini: '👯',
  Cancer: '🦀',
  Leo: '🦁',
  Virgo: '🌾',
  Libra: '⚖️',
  Scorpio: '🦂',
  Sagittarius: '🏹',
  Capricorn: '🐐',
  Aquarius: '🌊',
  Pisces: '🐟'
};

const zodiacDates = {
  Aries: 'March 21 – April 19',
  Taurus: 'April 20 – May 20',
  Gemini: 'May 21 – June 20',
  Cancer: 'June 21 – July 22',
  Leo: 'July 23 – August 22',
  Virgo: 'August 23 – September 22',
  Libra: 'September 23 – October 22',
  Scorpio: 'October 23 – November 21',
  Sagittarius: 'November 22 – December 21',
  Capricorn: 'December 22 – January 19',
  Aquarius: 'January 20 – February 18',
  Pisces: 'February 19 – March 20'
};

const dailyFortunes = [
  "🌟 Great things are coming your way!",
  "🎯 Today is a perfect day to chase your goals.",
  "🪐 You are on the right path—trust yourself.",
  "💫 Someone you admire will recognize your efforts.",
  "🧘‍♂️ Take a moment to breathe—clarity will follow.",
  "🍀 Luck is on your side today—take the leap!"
];

export default function HoroscopeApp() {
  const [selectedSign, setSelectedSign] = useState('');
  const [menuSelection, setMenuSelection] = useState('menu');
  <select
  value={menuSelection}
  className={`px-4 py-2 rounded-md border shadow ${darkMode ? 'bg-gray-800 text-white border-gray-600' : 'bg-white text-black border-gray-300'}`}
  onChange={(e) => {
    const value = e.target.value;
    setMenuSelection('menu'); // Reset to default after selection
    if (value === 'about') {
      alert('🔍 About Us: This site provides fun, light-hearted daily and weekly horoscopes, zodiac compatibility, and fortunes to brighten your day. Created with love and cosmic energy! ✨');
    } else if (value !== 'menu') {
      alert(`${value.charAt(0).toUpperCase() + value.slice(1)} page coming soon!`);
    }
  }}
>
  <option value="menu">Menu</option>
  <option value="about">About</option>
  <option value="contact">Contact</option>
  <option value="faq">FAQ</option>
  <option value="terms">Terms of Use</option>
</select>

  const [message, setMessage] = useState('');
  const [isWeekly, setIsWeekly] = useState(false);
  const [birthMonth, setBirthMonth] = useState('');
  const [birthDay, setBirthDay] = useState('');
  const [compatibilitySign1, setCompatibilitySign1] = useState('');
  const [compatibilitySign2, setCompatibilitySign2] = useState('');
  const [compatibilityResult, setCompatibilityResult] = useState('');
  const [darkMode, setDarkMode] = useState(false);
  const [selectedDate, setSelectedDate] = useState('');
  const [fortune, setFortune] = useState('');

  const horoscopes = {
    Aries: { daily: 'You are energetic and confident today!', weekly: 'A week of opportunity awaits you.' },
    Taurus: { daily: 'Stay grounded and focus on routine.', weekly: 'Productivity and patience will serve you well.' },
    Gemini: { daily: 'Communicate clearly and stay curious.', weekly: 'Connections will blossom this week.' },
    Cancer: { daily: 'Embrace your emotions and nurture yourself.', weekly: 'Family and home are your strength this week.' },
    Leo: { daily: 'Lead with your heart and confidence.', weekly: 'This is your week to shine bright.' },
    Virgo: { daily: 'Details matter—be precise and organized.', weekly: 'Plan and execute with intention.' },
    Libra: { daily: 'Balance and harmony bring success.', weekly: 'Relationship dynamics will improve.' },
    Scorpio: { daily: 'Trust your instincts—they guide you well.', weekly: 'Embrace transformation and change.' },
    Sagittarius: { daily: 'Explore and expand your horizons.', weekly: 'Adventure and learning await.' },
    Capricorn: { daily: 'Your discipline is your strength today.', weekly: 'Hard work will yield rewards.' },
    Aquarius: { daily: 'Innovate and inspire others today.', weekly: 'Your unique ideas gain attention.' },
    Pisces: { daily: 'Dream big and express your creativity.', weekly: 'Emotional growth and insight flourish.' }
  };

  const compatibilityMatrix = {
    Aries: ['Leo', 'Sagittarius', 'Gemini'],
    Taurus: ['Virgo', 'Capricorn', 'Cancer'],
    Gemini: ['Libra', 'Aquarius', 'Aries'],
    Cancer: ['Scorpio', 'Pisces', 'Taurus'],
    Leo: ['Aries', 'Sagittarius', 'Libra'],
    Virgo: ['Taurus', 'Capricorn', 'Cancer'],
    Libra: ['Gemini', 'Aquarius', 'Leo'],
    Scorpio: ['Cancer', 'Pisces', 'Virgo'],
    Sagittarius: ['Aries', 'Leo', 'Aquarius'],
    Capricorn: ['Taurus', 'Virgo', 'Pisces'],
    Aquarius: ['Gemini', 'Libra', 'Sagittarius'],
    Pisces: ['Cancer', 'Scorpio', 'Capricorn']
  };

  const getArchivedMessage = (sign) => {
    if (!sign || !selectedDate) return '';
    const day = new Date(selectedDate).getDate();
    const index = day % 2 === 0 ? 'daily' : 'weekly';
    return horoscopes[sign] ? horoscopes[sign][index] : '';
  };

  const getZodiacSign = (month, day) => {
    const date = new Date(2000, month - 1, day);
    const signs = [
      { sign: 'Capricorn', lastDay: new Date(2000, 0, 19) },
      { sign: 'Aquarius', lastDay: new Date(2000, 1, 18) },
      { sign: 'Pisces', lastDay: new Date(2000, 2, 20) },
      { sign: 'Aries', lastDay: new Date(2000, 3, 19) },
      { sign: 'Taurus', lastDay: new Date(2000, 4, 20) },
      { sign: 'Gemini', lastDay: new Date(2000, 5, 20) },
      { sign: 'Cancer', lastDay: new Date(2000, 6, 22) },
      { sign: 'Leo', lastDay: new Date(2000, 7, 22) },
      { sign: 'Virgo', lastDay: new Date(2000, 8, 22) },
      { sign: 'Libra', lastDay: new Date(2000, 9, 22) },
      { sign: 'Scorpio', lastDay: new Date(2000, 10, 21) },
      { sign: 'Sagittarius', lastDay: new Date(2000, 11, 21) },
      { sign: 'Capricorn', lastDay: new Date(2000, 11, 31) }
    ];
    for (const { sign, lastDay } of signs) {
      if (date <= lastDay) return sign;
    }
    return 'Capricorn';
  };

  const handleClick = (sign) => {
    if (horoscopes[sign]) {
      setSelectedSign(sign);
      const type = isWeekly ? 'weekly' : 'daily';
      const archiveMessage = getArchivedMessage(sign);
      setMessage(selectedDate ? archiveMessage : horoscopes[sign][type]);
    } else {
      setSelectedSign('');
      setMessage('');
    }
  };

  const handleBirthSubmit = () => {
    const sign = getZodiacSign(Number(birthMonth), Number(birthDay));
    handleClick(sign);
  };

  const checkCompatibility = () => {
    if (compatibilitySign1 && compatibilitySign2) {
      const compatible = compatibilityMatrix[compatibilitySign1] || [];
      const result = compatible.includes(compatibilitySign2)
        ? `💖 Yes! ${compatibilitySign1} and ${compatibilitySign2} are highly compatible!`
        : `⚠️ ${compatibilitySign1} and ${compatibilitySign2} may face some challenges.`;
      setCompatibilityResult(result);
    }
  };

  return (
    <div className={`min-h-screen transition-colors duration-500 ${darkMode ? 'bg-gradient-to-b from-gray-900 via-black to-gray-800 text-white bg-stars' : 'bg-gradient-to-br from-purple-200 via-indigo-100 to-white text-black'} max-w-4xl mx-auto p-6 space-y-8`}>
      <div className="flex justify-between items-center">
        <h1 className="text-4xl font-bold text-center drop-shadow">🔮 Daily & Weekly Horoscope 🔮</h1>
        <div className="relative">
          <select className={`px-4 py-2 rounded-md border shadow ${darkMode ? 'bg-gray-800 text-white border-gray-600' : 'bg-white text-black border-gray-300'}`} onChange={(e) => {
              const value = e.target.value;
              if (value === 'about') {
                alert('🔍 About Us: This site provides fun, light-hearted daily and weekly horoscopes, zodiac compatibility, and fortunes to brighten your day. Created with love and cosmic energy! ✨');
              } else {
                alert(`${value.charAt(0).toUpperCase() + value.slice(1)} page coming soon!`);
              }
            }}> 
            <option>Menu</option>
            <option value="about">About</option>
            <option value="contact">Contact</option>
            <option value="faq">FAQ</option>
            <option value="terms">Terms of Use</option>
          </select>
        </div>
      </div>

      <div className="flex justify-center items-center gap-4">
        <span className="text-md">Daily</span>
        <Switch checked={isWeekly} onCheckedChange={setIsWeekly} />
        <span className="text-md">Weekly</span>
        <span className="ml-6 text-md">Dark Mode</span>
        <Switch checked={darkMode} onCheckedChange={setDarkMode} />
      </div>

      <div className="flex justify-center gap-2 items-center">
        <input
          type="date"
          value={selectedDate}
          onChange={(e) => setSelectedDate(e.target.value)}
          className={`px-2 py-1 rounded ${darkMode ? 'bg-gray-800 text-white border-gray-600' : 'bg-white text-black border-gray-300'}`}
        />
        <span className={`text-sm ${darkMode ? 'text-gray-300' : 'text-gray-700'}`}>(Date selector for future archive use)</span>
      </div>

      <div className="flex justify-center gap-2 items-center">
        <input
          type="number"
          placeholder="Month"
          value={birthMonth}
          onChange={(e) => setBirthMonth(e.target.value)}
          className={`w-20 px-2 py-1 rounded ${darkMode ? 'bg-gray-800 text-white border-gray-600' : 'bg-white text-black border-gray-300'}`}
        />
        <input
          type="number"
          placeholder="Day"
          value={birthDay}
          onChange={(e) => setBirthDay(e.target.value)}
          className={`w-20 px-2 py-1 rounded ${darkMode ? 'bg-gray-800 text-white border-gray-600' : 'bg-white text-black border-gray-300'}`}
        />
        <Button onClick={handleBirthSubmit}>Find My Sign</Button>
      </div>

      <div className="flex flex-col sm:flex-row justify-center items-center gap-4">
        <select value={compatibilitySign1} onChange={(e) => setCompatibilitySign1(e.target.value)} className={`px-2 py-1 rounded ${darkMode ? 'bg-gray-800 text-white border-gray-600' : 'bg-white text-black border-gray-300'}`}>
          <option value="">Select First Sign</option>
          {Object.keys(zodiacEmojis).map(sign => <option key={sign}>{sign}</option>)}
        </select>
        <select value={compatibilitySign2} onChange={(e) => setCompatibilitySign2(e.target.value)} className={`px-2 py-1 rounded ${darkMode ? 'bg-gray-800 text-white border-gray-600' : 'bg-white text-black border-gray-300'}`}>
          <option value="">Select Second Sign</option>
          {Object.keys(zodiacEmojis).map(sign => <option key={sign}>{sign}</option>)}
        </select>
        <Button onClick={checkCompatibility}>Check Compatibility</Button>
      </div>

      {compatibilityResult && (
        <div className={`text-center text-lg font-medium ${darkMode ? 'text-white' : 'text-black'}`}>{compatibilityResult}</div>
      )}

      <div className="text-center">
        <Button onClick={() => setFortune(dailyFortunes[Math.floor(Math.random() * dailyFortunes.length)])}>
          Pull a Fortune ✨
        </Button>
        {fortune && (
          <div className={`mt-4 text-xl font-semibold ${darkMode ? 'text-white' : 'text-black'}`}>{fortune}</div>
        )}
      </div>

      <div className="grid grid-cols-2 sm:grid-cols-3 md:grid-cols-4 gap-6">
        {Object.entries(zodiacDates).map(([sign, dates]) => (
          <Card
            key={sign}
            onClick={() => handleClick(sign)}
            className={`cursor-pointer hover:shadow-xl hover:scale-105 transition-transform duration-300 ${darkMode ? 'bg-gray-800/70 border-gray-600' : 'bg-white/70 border'} backdrop-blur rounded-xl`}
          >
            <CardContent className="p-4 text-center space-y-2">
              <div className="text-3xl">{zodiacEmojis[sign]}</div>
              <div className={`text-xl font-semibold ${darkMode ? 'text-white' : 'text-black'}`}>{sign}</div>
              <div className={`text-sm ${darkMode ? 'text-gray-300' : 'text-gray-600'}`}>{dates}</div>
            </CardContent>
          </Card>
        ))}
      </div>

      {message && selectedSign && (
        <motion.div
          initial={{ opacity: 0, y: 10 }}
          animate={{ opacity: 1, y: 0 }}
          className={`text-2xl text-center font-medium ${darkMode ? 'bg-black/60 text-white' : 'bg-white/60'} p-4 rounded-xl shadow-md backdrop-blur`}
        >
          {zodiacEmojis[selectedSign]} {selectedSign}: {message}
        </motion.div>
      )}
    </div>
  );
}
