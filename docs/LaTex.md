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
\usepackage{listings} 
\usepackage{xcolor}
\usepackage{caption}
\usepackage{subfigure}
\usepackage{amsmath}
\usepackage{makecell}
\usepackage{url}
\lstset{
  frame=shadowbox, %把代码用带有阴影的框圈起来
  rulesepcolor=\color{red!20!green!20!blue!20},%代码块边框为淡青色
  keywordstyle=\color{blue!90}\bfseries, %代码关键字的颜色为蓝色，粗体
  commentstyle=\color{red!10!green!70}\textit,    % 设置代码注释的颜色
  showstringspaces=false,%不显示代码字符串中间的空格标记
  numbers=left, % 显示行号
  numberstyle=\tiny,    % 行号字体
  stringstyle=\ttfamily, % 代码字符串的特殊格式
  breaklines=true, %对过长的代码自动换行
  extendedchars=false,  %解决代码跨页时，章节标题，页眉等汉字不显示的问题
  %escapebegin=\begin{CJK*},escapeend=\end{CJK*},      % 代码中出现中文必须加上，否则报错
  texcl=true}
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

%\begin{lstlisting}[language=c++]
%	// example
%	#include <iostream>
%	
%	int main()
%	{
%		return 0;
%	}
%\end{lstlisting}

\section{Section}


\end{CJK}
\end{document}
```

