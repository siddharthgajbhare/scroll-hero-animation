# scroll-hero-animation
//Developed a premium scroll-driven hero animation using Next.js, Tailwind CSS, and GSAP ScrollTrigger. Implemented pinned scroll storytelling, scroll-synchronized motion, staggered intro animations, and parallax effects while optimizing performance using transform-based GPU-accelerated animations.

//code:-
"use client";
import { useEffect, useRef } from "react";
import gsap from "gsap";
import { ScrollTrigger } from "gsap/ScrollTrigger";

gsap.registerPlugin(ScrollTrigger);

export default function Hero() {
  const heroRef = useRef(null);
  const headlineRef = useRef(null);
  const statsRef = useRef(null);
  const carRef = useRef(null);
  const bgMidRef = useRef(null);
  const bgFrontRef = useRef(null);

  useEffect(() => {
    const ctx = gsap.context(() => {
      // Intro Animation
      const intro = gsap.timeline();
      intro.from(headlineRef.current.querySelectorAll(".letter"), {
        y: 100,
        opacity: 0,
        stagger: 0.03,
        duration: 1.1,
        ease: "power4.out",
      });
      intro.from(
        statsRef.current.children,
        {
          y: 50,
          opacity: 0,
          stagger: 0.2,
          duration: 0.6,
          ease: "power3.out",
        },
        "-=0.6"
      );
      // Scroll Animations
      gsap.timeline({
        scrollTrigger: {
          trigger: heroRef.current,
          start: "top top",
          end: "+=1600",
          scrub: 1.5,
          pin: true,
        },
      })
        // Background Mid Parallax
        .to(bgMidRef.current, {
          scale: 1.15,
          y: -250,
          ease: "none",
        })
        // Front Foreground Parallax
        .to(
          bgFrontRef.current,
          {
            x: -120,
            ease: "none",
          },
          0
        )
        // Car Movement
        .to(
          carRef.current,
          {
            x: 600,
            y: -20,
            rotation: 2,
            scale: 1.15,
            ease: "none",
          },
          0
        );
    }, heroRef);

    return () => ctx.revert();
  }, []);

  const headlineText = "WELCOME ITZFIZZ";

  return (
    <section
      ref={heroRef}
      className="relative h-screen w-full overflow-hidden bg-black text-white"
    >
      {/* BACKGROUND LAYERS */}
      <div
        ref={bgMidRef}
        className="absolute inset-0 bg-gradient-to-br from-gray-900 via-black to-gray-800"
      />
      <div
        ref={bgFrontRef}
        className="absolute inset-0 mix-blend-overlay"
      />
      {/* MAIN CONTENT */}
      <div className="relative z-20 flex flex-col items-center pt-28">
        <h1
          ref={headlineRef}
          className="text-center font-bold tracking-[0.6em] text-5xl md:text-7xl"
        >
          {headlineText.split("").map((c, i) => (
            <span key={i} className="letter inline-block">
              {c}
            </span>
          ))}
        </h1>
        <div
          ref={statsRef}
          className="flex gap-10 mt-8 text-center"
        >
          <div>
            <h2 className="text-3xl font-bold">95%</h2>
            <p className="text-sm text-gray-400">
              Performance
            </p>
          </div>
          <div>
            <h2 className="text-3xl font-bold">80%</h2>
            <p className="text-sm text-gray-400">
              Load Speed
            </p>
          </div>
          <div>
            <h2 className="text-3xl font-bold">100%</h2>
            <p className="text-sm text-gray-400">
              Experience
            </p>
          </div>
        </div>
        {/* CAR */}
        <img
          ref={carRef}
          src="/car.png"
          alt="Car"
          className="absolute bottom-20 left-[-280px] w-[520px] will-change-transform"
        />
      </div>
    </section>
  );
}
