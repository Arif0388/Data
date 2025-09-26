import 'package:flutter/material.dart';
import 'package:get/get.dart';
import 'package:google_fonts/google_fonts.dart';

class LoginPageWeb extends StatelessWidget {
  const LoginPageWeb({super.key});

  @override
  Widget build(BuildContext context) {
    final width = MediaQuery.of(context).size.width;

    return Scaffold(
      body: Row(
        children: [
          // Left Side (Images / Banner)
          Expanded(
            flex: 2,
            child: Container(
              height: double.infinity,
              decoration: const BoxDecoration(
                gradient: LinearGradient(
                  colors: [Color(0xff9C8DDF), Color(0xff856CF6)],
                  begin: Alignment.topLeft,
                  end: Alignment.bottomRight,
                ),
              ),
              child: Stack(
                children: [
                  Positioned(
                    top: 60,
                    left: 100,
                    child: Row(
                      children: [
                        Text("Future",
                            style: GoogleFonts.kanit(
                              fontWeight: FontWeight.w900,
                              color: Colors.white,
                              fontSize: 40,
                            )),
                        Text("skill",
                            style: GoogleFonts.kanit(
                              fontWeight: FontWeight.w900,
                              color: const Color(0xffDCC2FF),
                              fontSize: 40,
                            )),
                      ],
                    ),
                  ),
                  Center(
                    child: ClipRRect(
                      borderRadius: BorderRadius.circular(30),
                      child: Image.asset(
                        "assets/images/work_image3.jpg",
                        width: width / 3,
                        fit: BoxFit.cover,
                      ),
                    ),
                  ),
                ],
              ),
            ),
          ),

          // Right Side (Login Form)
          Expanded(
            flex: 3,
            child: Center(
              child: Container(
                width: 400,
                padding: const EdgeInsets.all(30),
                decoration: BoxDecoration(
                  color: Colors.white,
                  borderRadius: BorderRadius.circular(20),
                  boxShadow: const [
                    BoxShadow(
                      blurRadius: 15,
                      color: Colors.black12,
                      offset: Offset(2, 2),
                    )
                  ],
                ),
                child: Column(
                  mainAxisSize: MainAxisSize.min,
                  children: [
                    Text("Login to your account",
                        style: GoogleFonts.kanit(
                            fontWeight: FontWeight.w800,
                            fontSize: 26,
                            color: const Color(0xff52458B))),
                    const SizedBox(height: 20),
                    _buildTextField("Enter Phone Number"),
                    const SizedBox(height: 15),
                    Text("OR",
                        style: GoogleFonts.kanit(
                            color: const Color(0xff9C8DDF),
                            fontWeight: FontWeight.w500,
                            fontSize: 18)),
                    const SizedBox(height: 15),
                    _buildTextField("Enter Organization ID"),
                    const SizedBox(height: 10),
                    Align(
                      alignment: Alignment.centerRight,
                      child: Text("Forgot Organization ID?",
                          style: GoogleFonts.poppins(
                              fontSize: 14, fontWeight: FontWeight.w500)),
                    ),
                    const SizedBox(height: 30),
                    InkWell(
                      onTap: () {
                        Get.toNamed("/home"); // navigation
                      },
                      child: Container(
                        height: 50,
                        decoration: BoxDecoration(
                          borderRadius: BorderRadius.circular(25),
                          color: const Color(0xff52458B),
                        ),
                        child: Center(
                          child: Text("LOGIN",
                              style: GoogleFonts.kanit(
                                  color: Colors.white,
                                  fontWeight: FontWeight.w700,
                                  fontSize: 18)),
                        ),
                      ),
                    ),
                  ],
                ),
              ),
            ),
          ),
        ],
      ),
    );
  }

  Widget _buildTextField(String hint) {
    return Container(
      padding: const EdgeInsets.symmetric(horizontal: 12),
      decoration: BoxDecoration(
        borderRadius: BorderRadius.circular(10),
        color: const Color(0xffEEEEEE),
        border: Border.all(color: const Color(0xffD9D9D9)),
      ),
      child: TextField(
        decoration: InputDecoration(
          hintText: hint,
          border: InputBorder.none,
        ),
      ),
    );
  }
}
