# LaTeX

A Simple template:

```latex
\documentclass{article}

\usepackage{graphicx}
\usepackage{CJKutf8}
\usepackage{indentfirst}
\setlength{\parindent}{2em}
\usepackage{geometry}
\usepackage{gensymb}
\usepackage{enumerate}
\usepackage{float}
\usepackage{longtable}
\newcommand{\RNum}[1]{\uppercase\expandafter{\romannumeral #1\relax}}
\def\celsius{\ensuremath{^\circ\hspace{-0.09em}\mathrm{C}}}
\geometry{a4paper, scale = 0.8}
\begin{document}
\begin{CJK}{UTF8}{gbsn}

\title{Title}
\author{Author}
\maketitle

\section{Section}

\begin{figure}[H]
	\centering
	\caption{Caption}
	\includegraphics[scale = 0.5]{img/img01}
\end{figure}

% Table
\begin{table}[H]
\setlength{\tabcolsep}{3mm}{
\renewcommand\arraystretch{2}
\begin{tabular}{|c|c|c|c|c|c|c|c|c|c|c|c|}
\hline

\end{tabular}
}
\end{table}

\end{CJK}
\end{document}
```