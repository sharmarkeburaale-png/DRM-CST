import React, { useState, useEffect, useRef } from "react";
import { ArrowRight, ChevronDown } from "lucide-react";

const App = () => {
  const [email, setEmail] = useState("");
  const [phone, setPhone] = useState("");
  const [submitted, setSubmitted] = useState(false);
  const [timeLeft, setTimeLeft] = useState({});
  const [isMuted, setIsMuted] = useState(true);
  const [showScrollIndicator, setShowScrollIndicator] = useState(true);
  const videoRef = useRef(null);

  useEffect(() => {
    const calculateTimeLeft = () => {
      const launchDate = new Date("December 12, 2025 00:00:00").getTime();
      const now = new Date().getTime();
      const difference = launchDate - now;
      if (difference <= 0) return { expired: true };
      const days = Math.floor(difference / (1000 * 60 * 60 * 24));
      const hours = Math.floor((difference % (1000 * 60 * 60 * 24)) / (1000 * 60 * 60));
      const minutes = Math.floor((difference % (1000 * 60 * 60)) / (1000 * 60));
      const seconds = Math.floor((difference % (1000 * 60)) / 1000);
      return { days, hours, minutes, seconds };
    };
    setTimeLeft(calculateTimeLeft());
    const timer = setInterval(() => setTimeLeft(calculateTimeLeft()), 1000);
    return () => clearInterval(timer);
  }, []);

  const handleSubmit = (e) => {
    e.preventDefault();
    setSubmitted(true);
    setEmail("");
    setPhone("");
  };

  const scrollToForm = () => {
    document.getElementById("waitlist")?.scrollIntoView({ behavior: "smooth" });
  };

  const toggleMute = () => {
    if (videoRef.current) {
      videoRef.current.muted = !isMuted;
      setIsMuted(!isMuted);
    }
  };

  useEffect(() => {
    const handleScroll = () => {
      setShowScrollIndicator(window.scrollY <= 100);
    };
    window.addEventListener("scroll", handleScroll);
    return () => window.removeEventListener("scroll", handleScroll);
  }, []);

  return (
    <div className="min-h-screen bg-black text-white font-sans">
      {/* Hero Section — Fully Centered */}
      <section className="relative py-12 px-4 overflow-hidden">
        {/* Centered Logo */}
        <div className="flex justify-center mb-8">
          <img 
            src="https://i.ibb.co/dJ16j0Kw/IMG-7319.png" 
            alt="DRM CST Star Logo"
            className="w-28 h-28 md:w-32 md:h-32"
            style={{ filter: 'drop-shadow(0 0 8px #1dd1a1)' }}
          />
        </div>

        {/* HEAVY. PRECISION. UNAPOLOGETIC. */}
        <div className="text-4xl md:text-6xl font-bold leading-tight text-center mb-10 max-w-3xl mx-auto">
          HEAVY.<br />
          PRECISION.<br />
          UNAPOLOGETIC.
        </div>

        {/* Countdown + Video Side-by-Side — Perfect Top Alignment */}
        <div className="max-w-6xl mx-auto grid md:grid-cols-2 gap-6 items-start">
          {/* Countdown */}
          <div className="bg-gray-900 rounded-xl p-5 border border-gray-800 w-full">
            <div className="flex justify-between items-center mb-4">
              <h3 className="text-base font-bold">LAUNCH COUNTDOWN</h3>
              <div className="flex items-center gap-1 text-red-500 font-bold">
                <div className="w-2.5 h-2.5 bg-red-500 rounded-full"></div>
                LIVE
              </div>
            </div>
            {timeLeft.expired ? (
              <div className="text-lg font-bold text-[#1dd1a1] text-center py-4">LAUNCHING NOW!</div>
            ) : (
              <div className="grid grid-cols-4 gap-2 text-center">
                {["days", "hours", "minutes", "seconds"].map((unit, i) => (
                  <div 
                    key={i} 
                    className="bg-black rounded-lg p-3 border border-gray-800 hover:border-[#14bfa1] hover:shadow-[0_0_12px_#1df2c1] transition-all"
                  >
                    <div 
                      className="text-xl md:text-2xl font-bold text-[#1df2c1]" 
                      style={{ textShadow: "0 0 6px #1dd1a1, 0 0 12px #1dd1a1" }}
                    >
                      {timeLeft[unit] || "00"}
                    </div>
                    <div className="text-xs text-gray-400 mt-1">{unit.toUpperCase()}</div>
                  </div>
                ))}
              </div>
            )}

            {/* Waitlist Button — Placed directly below countdown, matching SECURE EARLY ACCESS color */}
            <div className="mt-6">
              <button
                onClick={scrollToForm}
                className="inline-flex items-center gap-2 text-lg font-bold group bg-[#1dd1a1] text-black px-6 py-3 rounded-lg transition-all duration-300 hover:bg-opacity-100 hover:shadow-[0_0_12px_#1df2c1]"
                style={{ textShadow: '0 0 6px #1dd1a1, 0 0 12px #1dd1a1' }}
              >
                👉 WAITLIST — 24H EARLY ACCESS →
                <ArrowRight className="w-5 h-5 group-hover:translate-x-1 transition-transform" />
              </button>
            </div>
          </div>

          {/* Video */}
          <div className="relative rounded-2xl overflow-hidden shadow-xl w-full aspect-video">
            <video
              ref={videoRef}
              autoPlay
              muted={isMuted}
              loop
              playsInline
              preload="auto"
              className="w-full h-full object-cover"
            >
              <source
                src="https://liue80szpwhht51b.public.blob.vercel-storage.com/1759615100008490.mp4"
                type="video/mp4"
              />
            </video>
            <button
              onClick={toggleMute}
              className="absolute top-3 right-3 z-10 bg-black bg-opacity-60 rounded-full p-1.5 hover:bg-opacity-80 text-white text-xs"
            >
              {isMuted ? "Unmute" : "Mute"}
            </button>
          </div>
        </div>

        {showScrollIndicator && (
          <div className="absolute bottom-6 left-1/2 transform -translate-x-1/2 animate-bounce">
            <ChevronDown className="w-7 h-7 text-gray-500" />
          </div>
        )}
      </section>

      {/* About Us, Our Mission, Our Culture Section */}
      <section className="py-16 px-4 bg-black">
        <div className="max-w-7xl mx-auto grid md:grid-cols-3 gap-6">
          {/* About Us */}
          <div className="bg-gray-900 rounded-xl p-5 border border-gray-800 hover:border-[#14bfa1] hover:shadow-[0_0_12px_#1df2c1] transition-all group cursor-pointer">
            <div className="aspect-square mb-3 overflow-hidden rounded-lg">
              <img
                src="https://i.ibb.co/sJkZ21g2/IMG-5239.jpg"
                alt="About Us"
                className="w-full h-full object-cover group-hover:scale-105 transition-transform duration-300"
                loading="lazy"
              />
            </div>
            <h3 className="text-lg font-semibold mb-1.5">About Us</h3>
            <p className="text-gray-300 text-sm">
              We redefine streetwear, merging artistry with exclusivity to create a lifestyle that resonates with the bold and the visionary.
            </p>
          </div>

          {/* Our Mission */}
          <div className="bg-gray-900 rounded-xl p-5 border border-gray-800 hover:border-[#14bfa1] hover:shadow-[0_0_12px_#1df2c1] transition-all group cursor-pointer">
            <div className="aspect-square mb-3 overflow-hidden rounded-lg">
              <img
                src="https://i.ibb.co/N2SNDqp7/IMG-5336.jpg"
                alt="Our Mission"
                className="w-full h-full object-cover group-hover:scale-105 transition-transform duration-300"
                loading="lazy"
              />
            </div>
            <h3 className="text-lg font-semibold mb-1.5">Our Mission</h3>
            <p className="text-gray-300 text-sm">
              Not for everyone. We craft timeless streetwear that challenges fast fashion’s noise. Limited runs. Premium fabrics. No restocks.
            </p>
          </div>

          {/* Our Culture */}
          <div className="bg-gray-900 rounded-xl p-5 border border-gray-800 hover:border-[#14bfa1] hover:shadow-[0_0_12px_#1df2c1] transition-all group cursor-pointer">
            <div className="aspect-square mb-3 overflow-hidden rounded-lg">
              <img
                src="https://i.ibb.co/b54SftVT/IMG-5384.jpg"
                alt="Our Culture"
                className="w-full h-full object-cover group-hover:scale-105 transition-transform duration-300"
                loading="lazy"
              />
            </div>
            <h3 className="text-lg font-semibold mb-1.5">Our Culture</h3>
            <p className="text-gray-300 text-sm">
              From Birmingham streets to global recognition, DRM CST celebrates individuality, authenticity, and the community that inspires our limited drops.
            </p>
          </div>
        </div>
      </section>

      {/* Waitlist Form */}
      <section id="waitlist" className="py-16 px-4">
        <div className="max-w-2xl mx-auto bg-gray-900 rounded-xl p-6 md:p-8 border border-gray-800">
          {!submitted ? (
            <form onSubmit={handleSubmit} className="space-y-5">
              <input
                type="email"
                placeholder="Email"
                value={email}
                onChange={(e) => setEmail(e.target.value)}
                required
                className="w-full px-3 py-2 bg-black border border-gray-700 rounded-lg text-white placeholder-gray-400 focus:ring-2 focus:ring-[#1dd1a1] focus:border-transparent transition-all duration-300"
              />
              <input
                type="tel"
                placeholder="Phone (optional)"
                value={phone}
                onChange={(e) => setPhone(e.target.value)}
                className="w-full px-3 py-2 bg-black border border-gray-700 rounded-lg text-white placeholder-gray-400 focus:ring-2 focus:ring-[#1dd1a1] focus:border-transparent transition-all duration-300"
              />
              <button
                className="w-full bg-[#1dd1a1] text-black font-bold py-3 px-4 rounded-lg transition-all duration-300 hover:bg-opacity-100 hover:shadow-[0_0_12px_#1df2c1]"
                style={{ textShadow: '0 0 6px #1dd1a1, 0 0 12px #1dd1a1' }}
              >
                SECURE EARLY ACCESS
              </button>
            </form>
          ) : (
            <div className="text-center">
              <div className="w-12 h-12 bg-[#1dd1a1] rounded-full flex items-center justify-center mx-auto mb-4">
                <span className="text-black text-lg">✓</span>
              </div>
              <p className="text-gray-300">You’re in. First access drops Dec 12. No spam. Just dreams.</p>
            </div>
          )}
        </div>
      </section>

      {/* Footer */}
      <footer className="py-6 px-4 border-t border-gray-800 text-center text-gray-600 text-xs">
        <p>DRM CST • 12.12.2025 • Birmingham, England</p>
      </footer>
    </div>
  );
};

export default App;
