# Digital Chama

A comprehensive platform for managing chama groups, contributions, and financial activities.

## Project Structure

This project consists of two main parts:

1. **Frontend (client)**: A Next.js application with React, TypeScript, and Tailwind CSS
2. **Backend (server)**: A Node.js Express API with TypeScript and Prisma ORM

## Getting Started

### Prerequisites

- Node.js (v16 or later)
- npm or yarn
- PostgreSQL (if using a local database)

### Installation

1. Clone the repository:

```bash
git clone https://github.com/your-username/digital-chama.git
cd digital-chama
```

2. Install dependencies:

```bash
# Install root dependencies
npm install

# Install client dependencies
cd client
npm install

# Install server dependencies
cd ../server
npm install
```

3. Set up environment variables:

- Copy `.env.example` to `.env` in the server directory and update the values
- Copy `.env.local.example` to `.env.local` in the client directory (if needed)

4. Start the development servers:

**On Windows:**
```bash
# From the project root
start-dev.bat
```

**On macOS/Linux:**
```bash
# From the project root
sh start-dev.sh
```

Or manually:
```bash
# From the project root
npm run dev
```

This will start both the client (on port 3000) and server (on port 5000) concurrently.

## Features

- User authentication and profile management
- Chama group creation and management
- Contribution tracking and management
- Loan application and approval processes
- Financial reports and analytics
- Meeting scheduling and management
- Group chat and announcements
- And much more!

## Technologies Used

### Frontend
- Next.js
- React
- TypeScript
- Tailwind CSS
- Shadcn UI Components

### Backend
- Node.js
- Express
- TypeScript
- Prisma ORM
- PostgreSQL
- Socket.IO

## License

This project is licensed under the MIT License.

## Acknowledgements

- All contributors and maintainers
- The open-source community for the amazing tools and libraries 