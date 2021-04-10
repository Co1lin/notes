# LaTeX

A Simple template:

```latex
\documentclass{article}

\usepackage{graphicx}
\usepackage{graphics}
\usepackage{CJKutf8}
\usepackage{indentfirst}
\setlength{\parindent}{2em}
\usepackage{geometry}
\usepackage{gensymb}
\usepackage{enumerate}
\usepackage{float}
\usepackage{longtable}
\usepackage{caption}
\usepackage{subfigure}
\usepackage{amsmath}
\usepackage{makecell}
\newcommand{\RNum}[1]{\uppercase\expandafter{\romannumeral #1\relax}}
\def\celsius{\ensuremath{^\circ\hspace{-0.09em}\mathrm{C}}}
\geometry{a4paper, scale = 0.8}
\begin{document}
\begin{CJK}{UTF8}{gbsn}

\title{LaTeX Example}
\author{Colin}
\maketitle

%	\begin{figure}[H]
%		\centering
%		\includegraphics[scale = 0.5]{img/curve04}
%		\caption{B - x 分布曲线}
%	\end{figure}

%\begin{figure}[H]
%	\centering
%	\subfigure[]{
%		\includegraphics[scale = 0.13]{img/exp124}
%	}
%	\subfigure[]{
%		\includegraphics[scale = 0.13]{img/exp3}
%	}
%	\subfigure[]{
%		\includegraphics[scale = 0.1]{img/exp6}
%	}
%\end{figure}

%	\begin{table}[H]
%	\centering
%	\setlength{\tabcolsep}{3mm}{
%	\renewcommand\arraystretch{2}
%		\begin{tabular}{|c|c|c|c|c|c|c|c|c|c|}
%		\hline
%		序号   & 1     & 2     & 3     & 4     & 5   & 6     & 标准差        & $\overline{p_\leftrightarrow}$      & $a_\updownarrow$    \\ \hline
%		$p_\leftrightarrow$/° & 267.5 & 266.9 & 267.4 & 267.8 & 267 & 267.1 & 0.34302575 & 267.283333 & 195.1 \\ \hline
%		\end{tabular}
%		}
%	\end{table}

	
\section{Section}
	
	
\end{CJK}
\end{document}
```

