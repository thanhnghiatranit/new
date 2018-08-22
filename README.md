/**
 * @(#)T001.java 2017/09/14.
 *
 * Copyright(C) 2016 by FUJINET CO., LTD.
 *
 * Last_Update 2017/09/14.
 * Version 1.00.
 */
package fjs.cs.action;

import java.io.IOException;
import java.io.PrintWriter;

import javax.servlet.RequestDispatcher;
import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpSession;

import fjs.cs.common.Constants;
import fjs.cs.dao.MSTUSER_DAO;

/**
 * T001(Login)
 * 
 * @author TIEN-NTM
 * @version 1.00
 * 
 */
public class Login extends HttpServlet {

	private static final long serialVersionUID = 1L;

	/**
	 * Init man hinh
	 * 
	 * @param HttpServletRequest
	 *            req
	 * @param HttpServletResponse
	 *            resp
	 * @return RequestDispatcher
	 */
	protected void doGet(HttpServletRequest req, HttpServletResponse resp)
			throws ServletException, IOException {
		RequestDispatcher myRD = null;
		// Hien thi man hinh Login
		myRD = req.getRequestDispatcher(Constants.T001_LOGIN);
		myRD.forward(req, resp);
	}

	/**
	 * Event man hinh
	 * 
	 * @param HttpServletRequest
	 *            req
	 * @param HttpServletResponse
	 *            resp
	 * @return RequestDispatcher
	 */
	protected void doPost(HttpServletRequest req, HttpServletResponse resp)
			throws ServletException, IOException {
	
		String action=req.getParameter("mode");
		if(action.equals("login")){
			// tao session
			HttpSession session = req.getSession();
			PrintWriter out = resp.getWriter();
			String username = req.getParameter("txtUserID");
			String password = req.getParameter("txtPassword");
			// xac thuc id va pass
			if (MSTUSER_DAO.validate(username, password)) {
				session.setAttribute("txtUserID", username);
				RequestDispatcher rd = req
						.getRequestDispatcher(Constants.T002_SEARCH);
				rd.forward(req, resp);
			} else {
				RequestDispatcher rd = req
						.getRequestDispatcher(Constants.T001_LOGIN);
				rd.forward(req, resp);
			} 
		
		}
		if(action.equals("logout")){
			HttpSession session = req.getSession();
			PrintWriter out = resp.getWriter();
			session.invalidate();
			req.getRequestDispatcher(Constants.T001_LOGIN).include(req, resp);
			//resp.sendRedirect("jsp/T001.jsp");
			
			out.close();
		}

		
	}
}

