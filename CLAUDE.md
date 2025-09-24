# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Development Commands

- `npm run dev` - Start development server using Vite
- `npm run build` - Build for production
- `npm run lint` - Run ESLint for code linting
- `npm run preview` - Preview production build

## Environment Setup

- `.env` file required with OpenAI API key:
  - `VITE_GPT_KEY` or `OPENAI_API_KEY` for OpenAI DALL-E image generation
  - Get API key from: https://platform.openai.com/api-keys

## Project Architecture

This is a React + TypeScript hair salon style generator application built with Vite. The project structure is:

- Single-page application with main component in `src/App.tsx`
- Uses Tailwind CSS for styling with configuration in `tailwind.config.js`
- TypeScript configured with strict mode enabled
- ESLint configured with React hooks and TypeScript rules

### Main Application Features

The app (`src/App.tsx`) is a comprehensive hair styling tool with:

- **8-Category Hair Style System**: Structured hair customization with:
  - 髪の長さ (Hair Length) - 7 options from ロング to ベリーショート
  - シルエット (Silhouette) - 8 options including Aライン, Iライン, ひし形
  - パーマ (Perm) - Simple あり/なし selection
  - 髪の動き (Hair Movement) - 8 options from ナチュラルカール to くびれ
  - 質感 (Texture) - 10 options from 軽め to 透け感
  - 髪質 (Hair Quality) - 3 thickness options
  - レイヤーの構成 (Layer Structure) - 5 layering styles
  - ライティング (Lighting) - 3 lighting options
- **Color Chart System**: Interactive color selection with two modes:
  - Black hair base colors (`blackHairBaseColors`) - levels 3-10
  - Bleach base colors (`bleachBaseColors`) - levels 1-15
- **AI Image Generation**: Real-time image generation using OpenAI DALL-E API
- **Image Analysis Integration**: GPT-4o Vision analyzes uploaded images to understand current hairstyle and facial features
- **Image Upload**: File upload functionality for reference images
- **Favorites System**: Users can favorite generated styles

### Key Data Structures

- `ColorChart` interface defines color levels and codes
- 8 structured option arrays with Japanese labels and English values:
  - Each category maps Japanese UI labels to English prompt values
  - Example: "ロング" → "long hair", "Aライン" → "A-line silhouette"
- `generatePrompt()` function combines selections into "make her hairstyle [options] with [color] hair"
- Japanese language interface throughout

### Tech Stack

- React 18 with TypeScript
- Vite for bundling and development
- Tailwind CSS for styling
- Lucide React for icons
- OpenAI API for DALL-E image generation and GPT-4o Vision analysis
- ESLint with TypeScript and React plugins

## Notes

- No test framework is configured
- Requires OpenAI API key for image generation functionality
- Japanese language application for hair salon styling
- Color chart data is hardcoded with hex values for different hair color levels and tones
- Generates 4 images per request using DALL-E 3 model
- Browser-compatible OpenAI client (with `dangerouslyAllowBrowser: true`)

## Image Analysis & Prompt Generation System

The application uses a two-stage AI approach:

### Stage 1: Image Analysis (GPT-4o Vision)
- When user uploads an image, GPT-4o Vision analyzes:
  - Current hairstyle, hair color, and length
  - Face shape and pose
  - Background and makeup
  - Overall appearance details
- Analysis result is stored and displayed to user

### Stage 2: Enhanced Prompt Generation
1. **With uploaded image**: Uses analysis as base context: "Based on this person: [analysis]. Make her hairstyle [options]"
2. **Without image**: Standard format: "Make her hairstyle [comma-separated options]"
3. Appends hair color if selected: "with [color-code] hair"
4. Sends enhanced prompt to OpenAI DALL-E API for more accurate generation

### Key Functions
- `convertImageToBase64()`: Converts uploaded files to Base64 format
- `analyzeImageWithVision()`: Calls GPT-4o with image for detailed analysis
- `generatePrompt()`: Creates context-aware prompts based on analysis and selections